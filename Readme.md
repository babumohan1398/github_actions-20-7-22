* This is First assessment of the DevOps Assessment series
* The sample application in this repo will respond with ‘Hello World’ to any GET request
* A Jenkins CICD pipeline needs to be created to dockerize the application and deploy to the EKS cluster
* Assessment to be perfromed on your Personal AWS free account.
* An EKS cluster namespace will be provided to you for deployment

## Terrafrom

Create a terrafrom script to create the below resournces on your Free AWS account.

1) A new VPC of CIDR range - **10.0.0.0/16**
2) A public subnet with CIDR range - **10.0.0.0/19** within the same VPC 
3) Create a EC2 instance in the public subnet with a Elastic IP with **Name** tag as **Jenkins**
4) Ensure to create other network components to ensure the Inbound/Outboud communication from the instance works.
5) The state file is to stored in the S3 backend and manage lock file using dynamodb on the personal AWS Free account.

The terrafrom scripts are to be kept on the clops gitlab account and master branch is supposed to be submitted for review 

## Ansible

Perfrom the below activity using Ansible on the Jenkins EC2 machine

* Install **Jenkins**
* Install **Git** and **kubectl** on the same
* Install **Docker runtime environment**
* Install the openshift Python module

You can use the anisble-galaxy role to install Jenkins and docker. Install the openshift Python module to enable Ansible to connect with Kubernetes.

The ansible scripts are to be kept on the clops gitlab account and master branch is supposed to be submitted for review 

## Create a Docker file for the application

* Create a [multistage](https://docs.docker.com/develop/develop-images/multistage-build/) docker file for the application.  
* Starts a build image with [golang:alpine](https://hub.docker.com/_/golang).
* The resulting binary to be spun up using [scratch](https://hub.docker.com/_/scratch/) image
* Test the docker file and create the docker image out of it manually and name it at clops_go image

## Create the K8s manifest files

Create the below k8s manifest files for the deployment based on the requirements given below. You may refer the official documnetaion of k8s for the same.

##### 1) service.yml 
* Type - LoadBalancer
* Port to access the application - 80
* Namespace - Namespace "of your name" **(Do not use default namespace)**

##### 2) deployment.yml 
* replicas: 2
* resources (requests) of CPU - 10m
* Namespace - Namespace "of your name" **(Do not use default namespace)**

## Configuring Jenkins

* Create the kubeconfig file and use the appropriate plugin for k8s deployment.
* Add the credentials of both Gitlab and the docker repository that you are using to Jenkins.
* Make sure that you give a meaningful ID and description to each.

## Create the Jenkins CICD Pipeline Job

Create a pipeline type Jenkins job for the CICD process for this repo. Fork this repo to your Gitlab account and create a new pipeline for the same.

* The docker files is to be kept in the newly forked repp
* The k8s files are also to be kept in the new forked application repo for now.
* Use the Poll SCM as the build trigger for the CI process.
* The build branch should be develop.
* All the pipline code should be placed in a Jenkinsfile in the same repository.

The CI pipeline job should contain  below stages

1) Code Checkout
2) Docker Build
3) Publish to DockerHub
4) Deploy to EKS
5) Send Notification to your Cloudifyops email ID

For deployment stage - you can use either create a Ansible playbook for kubernetes deployment or use appropriate kubectl commands or even jenkins plugins.