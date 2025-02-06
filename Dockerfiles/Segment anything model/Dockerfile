# syntax=docker/dockerfile:1
ARG PYTHON_VERSION=3.10
ARG PORT=9090  # Added ARG for PORT
FROM python:${PYTHON_VERSION}-slim AS python-base
ARG TEST_ENV

WORKDIR /app

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PORT=${PORT} \
    PIP_CACHE_DIR=/.cache \
    WORKERS=1 \
    THREADS=8 \
    VITH_CHECKPOINT=/app/models/sam_vit_h_4b8939.pth \
    MOBILESAM_CHECKPOINT=/app/models/mobile_sam.pt \
    ONNX_CHECKPOINT=/app/models/sam_onnx_quantized_example.onnx

# Update the base OS
RUN --mount=type=cache,target="/var/cache/apt",sharing=locked \
    --mount=type=cache,target="/var/lib/apt/lists",sharing=locked \
    set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt install --no-install-recommends -y  \
        wget git libopencv-dev python3-opencv cmake protobuf-compiler; \
    apt-get autoremove -y

# Copy and run the model download script
COPY download_models.sh .
RUN sed -i 's/\r//' /app/download_models.sh

# install base requirements
COPY requirements-base.txt .
RUN --mount=type=cache,target=${PIP_CACHE_DIR},sharing=locked \
    pip install --upgrade pip && \
    pip install -r requirements-base.txt

# install custom requirements
COPY requirements.txt .
RUN --mount=type=cache,target=${PIP_CACHE_DIR},sharing=locked \
    pip install -r requirements.txt

# install test requirements if needed
COPY requirements-test.txt .
# build only when TEST_ENV="true"
RUN --mount=type=cache,target=${PIP_CACHE_DIR},sharing=locked \
    if [ "$TEST_ENV" = "true" ]; then \
      pip install -r requirements-test.txt; \
    fi

COPY . .
# Add this line before using curl
RUN apt-get update && apt-get install -y curl

RUN mkdir -p /app/models

RUN curl -o /app/models/sam_vit_h_4b8939.pth https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth


# Add ONNX model
RUN python3 onnxconverter.py

EXPOSE 9090

# Use JSON array for CMD to avoid shell issues
CMD ["gunicorn", "--bind", ":${PORT}", "--workers", "$WORKERS", "--threads", "$THREADS", "--timeout", "0", "_wsgi:app"]

