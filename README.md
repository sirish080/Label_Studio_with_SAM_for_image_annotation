# Label_Studio_with_SAM_for_image_annotation

1. Download the directory https://github.com/HumanSignal/label-studio-ml-backend.git and do the following steps:

    1. Replace the docker file of segment anything model located in label-studio-ml-backend\label_studio_ml\examples\segment_anything_model with the file for SAM located in folder Dockerfiles.
    2. Create another directory named label-studio in label-studio-ml-backend\label_studio_ml\examples\
    3. Copy the docker file for label-studio located in folder Dockerfiles  and and paste it to the label-studio directory created in step 2.

2. Open one terminal in command prompt and start building the docker image for SAM ML backend following the instructions in https://labelstud.io/blog/get-started-using-segment-anything/

3. Open another command prompt terminal, navigate to the directory where your Dockerfile for label-studio is located, and run the following command in the brackets to build the Docker image:( docker build -t label-studio-with-ml . )

4. After the label-studio image is successfully built, run it using the following command: docker run -it -p 8080:8080 -v <Put the path of directory label-studio created in step 1.2. Note: remove <> too.>/mydata:/label-studio/data label-studio-with-ml

By this time, Label-studio should be running in http://localhost:8080/

5. Open the docker-compose.yml file located in label-studio-ml-backend\label_studio_ml\examples\segment_anything_model and update your host ip address and label-studio access token. Make sure there is no space between the sign = and the info you updated. It should be as below:
                             - LABEL_STUDIO_HOST=http://192.168.1.1:8080
                             - LABEL_STUDIO_ACCESS_TOKEN=xzMxzMcM375JtFhXu9ZBScuGXa

6. Now go to the command prompt where you are building the SAM ML backend and run the command: docker compose up

7. Once the docker is ready, go to the label studio and create a project.
  
8. Go to project settings>model and add the model. The model name can be anything you want. The backend URL should be your local ip address:9090. For example: http://134.129.127.190:9090 . Make sure the interactive preannotations is tured on. Then Validate and save.

9. Go to the labelling interface and use the format specified in the blog of label studio (https://labelstud.io/blog/get-started-using-segment-anything/). copy and paste the following code and change as per your requirement.

10. Now you should be ready to start annotating with Label-studio with SAM.

11. Use Ctrl+c to close the label-studio and ML backend.




