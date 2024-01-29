# CICD Pipeline Compute Engine GCP

3 branches within the github repo: https://github.com/OmarHaka/cloud-Pythonweb


Create a docker container for your python web application. In that docker file install all requirements and run the application.


Create environment for pipeline
Steps are as follows:
1. One repository,containing three branches
2. First branch containing python application code and cloud-build YAML file. Here we have used the GitHub repository for demonstration.
3. Second branch containing the cloud-build YAML file for creating Instance-template containing container and MIG
4. Third branch containing the cloud-build YAML file for updating the existing MIG with a new version of instance-template.
5. Cloud build configured with GitHub application code repository to execute a build based on the events in the repository.
6. Artifact Registry to store the images built from cloud build.


3 Build triggers set to watch changes on each branch of the repository.
![image](https://github.com/ohakawati/CI-CD_ComputeEngine/assets/89810188/a24ce8d5-c91b-4e82-8564-857476b54917)


1.Continuous Integration(CI Pipeline):


-A Developer makes the changes in the code, fixes the bugs, and pushes it to the application repository.


-Cloud build then invokes triggers either manually or automatically by the events on the repository such as pushes or pull requests.


-Once the trigger gets invoked by any events, cloud build then executes the instructions written in the build config file (cloudbuild.yaml) such as building the docker image from Dockerfile provided and pushing it to the artifact registry configured.


-Once the image with the new tag got pushed to the Artifact Registry.it will get updated in the instance-template used for creating the managed instance group of the compute engine.


2.Continuous Deployment(CD Pipeline):


-Cloud build then invokes a trigger either manually or automatically by the events on the repository branch such as pushes or pull requests


-Once the trigger gets invoked by any events, cloud build then executes the instructions written in the build config file (cloudbuildcd.yaml) such as building the instance template containing the image stored in Artifact registry and creating the managed instance group with container running in it.


-Creating another trigger,Once the trigger gets invoked by any events,cloud build then executes updating your MIG written in the build config file (cloudbuildcd2.yaml). Now that your MIG is up there only this update build yaml will be needed in updating the MIG with a new version of instance-template. 
![image](https://github.com/ohakawati/CI-CD_ComputeEngine/assets/89810188/4dc33d3b-3774-4ed4-b722-0dd9e517cac7)


For Continuos integration yaml a load test is included but commented out. A report on the load test for the managed instance group is saved to a Google Cloud Storage bucket for review.


# Scaling Instances Based on CPU Utilization

By using apache Jmeter I was able to simulate a load to the application to display Googles autoscaling policy in action. Attaching the Autoscaling policy can be included as part of your build steps and be fined tuned to your desires. For this test I just used Googles default metric which is CPU utilization and thats set when enabling autoscaling on your MIG.

![image](https://github.com/ohakawati/CI-CD_ComputeEngine/assets/107427573/e4508cc1-098f-488b-aa56-4c76f3ae3021)

![image](https://github.com/ohakawati/CI-CD_ComputeEngine/assets/107427573/d4bfb1ef-bcc1-4c38-88eb-5b56772efc3d)
For this graphic I ended up lowering the target CPU util to 20% instead of 70% because the load wasnt big enough initially. You can see when the threshold is exceeded the MIG scaled out to 8 instances. 





