# Use the official Label Studio image as the base image
FROM heartexlabs/label-studio:latest

# Install label-studio-ml==1.0.9
RUN pip install label-studio-ml==1.0.9

# Set environment variables
ENV LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
ENV LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/label-studio/data/images

# Expose port 8080
EXPOSE 8080

# Start the Label Studio application
CMD ["label-studio"]
