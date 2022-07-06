* This is the First assessment of the DevOps Assessment series
* The sample application in this repo will respond with ‘Hello World’ to any GET request
* A Jenkins CICD pipeline needs to be created to dockerize the application and deploy it to the EKS cluster
* Assessment to be performed on your Personal AWS free account.
* An EKS cluster namespace will be provided to you for deployment

## Terraform

Create a terraform script to create the below resources on your Free AWS account.

1) A new VPC of CIDR range - **10.0.0.0/16**
2) A public subnet with a CIDR range - **10.0.0.0/19** within the same VPC 
3) Create an EC2 instance in the public subnet with an Elastic IP with **Name** tag as **Jenkins**
4) Ensure to create other network components to ensure the Inbound/Outbound communication from the instance works.
5) The state file is to be stored in the S3 backend and manage the lock file using dynamodb on the personal AWS Free account.

The terraform scripts are to be kept on the clops GitLab account and the master branch is supposed to be submitted for review 

## Ansible

Perform the below activity using Ansible on the Jenkins EC2 machine

* Install **Jenkins**
* Install **Git** and **kubectl** on the same
* Install **Docker runtime environment**
* Install the openshift Python module

You can use the ansible-galaxy role to install Jenkins and docker. Install the openshift Python module to enable Ansible to connect with Kubernetes.

The ansible scripts are to be kept on the clops GitLab account and the master branch is supposed to be submitted for review 

## Create Docker file for the application

* Create a [multistage](https://docs.docker.com/develop/develop-images/multistage-build/) docker file for the application.  
* Starts a build image with [golang:alpine](https://hub.docker.com/_/golang). Follow the below steps
    1) Create a dir /go/src/app
    2) Copy main.go into the above dir
    3) Make the above dir as the WORKDIR
    4) Run the below command from the above dir

    ```RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o app main.go ```
* The resulting binary to be spun up using [scratch](https://hub.docker.com/_/scratch/) image
    1) Entry point should be as below
    
    ```ENTRYPOINT [ "./app" ]```
* Test the docker file and create the docker image out of it manually and name it at clops_go image
## Create the K8s manifest files

Create the below k8s manifest files for the deployment based on the requirements given below. You may refer to the official documentation of k8s for the same.

##### 1) service.yml 
* Type - Load Balancer
* Port to access the application - 80
* Namespace - Namespace "of your name" **(Do not use default namespace) **

##### 2) deployment.yml 
* Replicas: 2
* Resources (requests) of CPU - 10m
* Namespace - Namespace "of your name" **(Do not use default namespace) **

## Configuring Jenkins

* Create the kubeconfig file and use the appropriate plugin for k8s deployment.
* Add the credentials of both Gitlab and the docker repository that you are using to Jenkins.
* Make sure that you give a meaningful ID and description to each.

## Create the Jenkins CICD Pipeline Job

Create a pipeline-type Jenkins job for the CICD process for this repo. Fork this repo to your Gitlab account and create a new pipeline for the same.

* The docker files are to be kept in the newly forked repp
* The k8s files are also to be kept in the new forked application repo for now.
* Use the Poll SCM as the build trigger for the CI process.
* The build branch should be develop.
* All the pipeline code should be placed in a Jenkinsfile in the same repository.

The CI pipeline job should contain the below stages

1) Code Checkout
2) Docker Build
3) Publish to DockerHub
4) Deploy to EKS
5) Send a Notification to your CloudifyOps email ID

For the deployment stage - you can use either create an Ansible playbook for Kubernetes deployment or use appropriate kubectl commands or even Jenkins plugins.
