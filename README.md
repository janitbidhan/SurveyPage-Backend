# SWE 645 Survey Page Pipeline Deployment

This pipeline deploys an application using Jenkins, Docker, and Kubernetes. It follows these stages:

Prerequisites: This stage sets up the necessary credentials and environment variables needed for the deployment.

Checkout: This stage checks out the source code from the SCM.

Build: This stage builds the application and archives the generated artifacts.

Docker Build and Push: This stage builds a Docker image of the application and pushes it to a Docker registry.

Deploy to Kubernetes: This stage deploys the application to a Kubernetes cluster by restarting the deployment.

The pipeline uses the following environment variables:

DOCKER_REGISTRY: The Docker registry used to store the Docker image.
DOCKER_CREDENTIALS: The credentials used to authenticate to the Docker registry.
KUBERNETES_NAMESPACE: The Kubernetes namespace used to deploy the application.
KUBERNETES_DEPLOYMENT_NAME: The name of the Kubernetes deployment used to deploy the application.
KUBERNETES_CONTAINER_NAME: The name of the Kubernetes container used to run the application.
KUBERNETES_CONTAINER_PORT: The port used by the Kubernetes container to expose the application.
The pipeline has been tested and deployed successfully in a production environment for the SWE 645 course.

>[Access it here on LoadBalancer ]([URL](https://18.209.26.76/k8s/clusters/c-ff5k8/api/v1/namespaces/swe-a2/services/http:deploy-a2:8080/proxy/))
>[JENKINS](http://107.23.40.143:8080/)
>[RANCHER](https://18.209.26.76/dashboard/)
