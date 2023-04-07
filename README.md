# SWE 645 Assignment 2: Survey Page Pipeline CI/CD
##Team
> Name: Janit Bidhan, Badri Nath Gaur
> 
> G Number:G01326011, G01330366
>  
> Email: [jbidhan@gmu.edu](jbidhan@gmu.edu), [bgaur@gmu.edu](bgaur@gmu.edu)

## Contents
These are better explained in the document in detail:
- Overview
- Tasks (Explained in document)
- Contributions
- Challenges 
- Pre-requisites for the assignment:
- Technology Utilised
- Jenkins Pipline Explanation
- Folder Structure

## Overview
For this assignment, our objective is to deploy a containerized application on Kubernetes to enhance its scalability and resilience. Our baseline configuration involves running at least three pods continuously.

To manage the Kubernetes services on GKE in the Google Cloud Platform, we utilize Rancher. Additionally, we have established a CI/CD pipeline that involves a Git source code repository on GitHub and Jenkins for automated build and deployment of the application on Kubernetes.

When changes are made to the code in the GitHub repository, the pipeline automatically triggers a build and deployment process to ensure that the latest version of the application is always available on Kubernetes.

>Note: These are as part of document, location can be seen in the folder Structure i.e. **SWE645-Assignment2.pdf**

>Also there is a video link at the end and the video in the folder itself.
##Tasks
- Part 1: Store Survey Page on Github, Dockerize the application using Docker and dockerhub.
- Part 2: Create EC2 instances using AWS lab for Jenkins and Rancher
- Part 3: Setup Google Cloud Platfrom for GKE
- Part 4: Setup up Rancher to manage and create Kubernetes cluster on GKE.
- Part 5: Manage CI CD using Jenkins, Docker Hub

# Contributions
> Badri Nath Gaur: 
> >Dockerized the application to containerize it, which enabled easy deployment and management on Kubernetes.
Set up Rancher on EC2, providing a user-friendly interface to manage Kubernetes services.
Configured the Kubernetes deployment, including defining the number of replicas, resource allocation, and networking settings.
> 
> Janit Bidhan: 
> >Managed Rancher on EC2, ensuring smooth operation of Kubernetes services.
Installed Jenkins and configured it to automate build and deployment processes.
Set up the pipeline in Jenkins, defining stages and steps such as compiling code, building the Docker image, pushing to the container registry, and updating the Kubernetes deployment.
>  
>  Common Tasks:
> >Collaborated to determine the best approach for containerizing and deploying the application on Kubernetes.
Determined the optimal configuration for the Kubernetes deployment, including resource allocation, networking, and scaling policies.
Set up access controls and permissions for the Rancher and Jenkins instances to ensure secure access and prevent unauthorized changes.
Monitored the performance and health of the Kubernetes deployment and made adjustments as needed to optimize efficiency and maintain stability.
Troubleshot any issues that arose during the deployment and worked together to resolve them quickly to minimize downtime.
Conducted testing and quality assurance to ensure that the application was functioning as expected and meeting performance requirements. 

## Challenges
>Troubleshooting issues with the Kubernetes deployment, especially when dealing with containerization and networking
> 
> Setting up and configuring access controls and permissions for Rancher and Jenkins
> 
> Making Rancher and Jenkins work together to perform Continues deployment.
>  
## Pre-requisites for the assignment:
1. IDE for development.
2. Docker Desktop for building and testing local image.
3. Access to AWS Learner lab provided by Dr. Vinod Dubey.
4. Personal accounts on GitHub, Google Cloud Platform and Docker Hub.


## Technology Utilised
1. Github
2. IntelliJ IDE
3. Docker Desktop and Docker Hub
4. AWS EC2, AWS Elastic IP
5. Google Cloud Platform (GKE)
6. Jenkins
7. Kubernetes
8. Rancher 


## Jenkins Pipline Explanation

This pipeline deploys an application using Jenkins, Docker, and Kubernetes. It follows these stages:

__Prerequisites__: This stage sets up the necessary credentials and environment variables needed for the deployment.

__Checkout__: This stage checks out the source code from the SCM.

__Build__: This stage builds the application and archives the generated artifacts.

__Docker Build and Push__: This stage builds a Docker image of the application and pushes it to a Docker registry.

__Deploy to Kubernetes__: This stage deploys the application to a Kubernetes cluster by restarting the deployment.

The pipeline uses the following environment variables:

- DOCKER_REGISTRY: The Docker registry used to store the Docker image.
- DOCKER_CREDENTIALS: The credentials used to authenticate to the Docker registry.
- KUBERNETES_NAMESPACE: The Kubernetes namespace used to deploy the application.
- KUBERNETES_DEPLOYMENT_NAME: The name of the Kubernetes deployment used to deploy the application.
- KUBERNETES_CONTAINER_NAME: The name of the Kubernetes container used to run the application.
- KUBERNETES_CONTAINER_PORT: The port used by the Kubernetes container to expose the application.

The pipeline has been tested and deployed successfully for the SWE 645 course. Any changes in the repo, triggers a jenkins build which replaces the docker image with the new build and deploys on docker.

>[Access it here on LoadBalancer ](http://35.202.219.90:8080/)
>
>[JENKINS](http://107.23.40.143:8080/)
>
>[RANCHER](https://18.209.26.76/dashboard/)
>
>[Youtube Private Video Link](https://youtu.be/ASzeKtW-gDk)
>  (Last 2 minutes show the demo of chnages made in github trigger the jenkins job to deploy.)

## Folder Structure
```
.
├── AWS
│   ├── Default Admin.yaml
│   ├── cluster-swe.yaml
│   ├── deploy-a2.yaml
│   ├── swe-645-assignment2-a57bb1b4da9c.json
│   ├── swe-645.pem
│   └── swe645-janit.pem
├── CI:CD Pipeline Demo and Setup.mp4
├── SWE645-Assignment2.pdf
├── SurveyPage-HW2
│   ├── Dockerfile
│   ├── Jenkinsfile
│   ├── META-INF
│   │   └── MANIFEST.MF
│   ├── README.md
│   ├── ROOT.war
│   ├── RancherYAMLFiles
│   │   ├── Cluster.yaml
│   │   └── Deployment.yaml
│   ├── pom.xml
│   └── src
│       ├── main
│       │   ├── java
│       │   ├── resources
│       │   └── webapp
│       │       ├── WEB-INF
│       │       │   └── web.xml
│       │       ├── assets
│       │       │   ├── css
│       │       │   │   └── styleSurvey.css
│       │       │   ├── img
│       │       │   │   ├── 404-error.png
│       │       │   │   ├── form-icon.jpg
│       │       │   │   └── imgThankYou.jpeg
│       │       │   └── js
│       │       │       └── main.js
│       │       ├── error
│       │       │   └── error.html
│       │       └── index.html
│       └── test
│           └── java
└── readme.md

18 directories, 25 files
```

