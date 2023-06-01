# Project 03 - Refactor Udagram App into Microservices and Deploy
### Overview - Udagram Image Filtering Microservice
The project application, Udagram - an Image Filtering application, allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice. It has two components:

Frontend - Angular web application built with Ionic framework
Backend RESTful API - Node-Express application

### Refactor Udagram App into Microservices and Deploy
1/ Create an S3 bucket
Create S3 Bucket
![Create S3 Bucket](./screenshots/bucket-s3.PNG)
S3 Bucket Detail
![Bucket Detail](./screenshots/bucket-s3-detail.PNG)
Bucket Properties
![Bucket Properties](./screenshots/bucket-s3-properties.PNG)
Bucket Permission
![Bucket Permission](./screenshots/bucket-s3-permission.PNG)
Bucket Cors
![Bucket CORS](./screenshots/bucket-s3-cors.PNG)
Buket Policy
![Bucket Policy](./screenshots/bucket-s3-policy.PNG)

2/ Create PostgreSQL database
Create PostgreSQL Database
![Create PostgreSQL Database](./screenshots/postgresql-database.PNG)
Connectivity & Security
![Connectivity & Security](./screenshots/postgresql-connectivity-security.PNG)
Configuration
![Configuration](./screenshots/postgresql-configuration.PNG)
3/ Create Cluster on Amazon Elastic Kubernetes Service
Create Elastic Kubernetes Service on Command line
`eksctl create cluster --name hungdoan-eks --region us-east-2 --version 1.26 --vpc-private-subnets subnet-0c809077c56bf4ab9,subnet-037fe91e18857c1fd,subnet-0c836399717f4b682 --without-nodegroup`
![Create EKS](./screenshots/create-eks-command-line.PNG)
Elastic Kubernetes Service
![Elastic Kubernetes Service](./screenshots/create-eks.PNG)
Elastic Kubernetes Service (EKS) Overview
![EKS Overview](./screenshots/eks-overview.PNG)
EKS Compute
![EKS Compute](./screenshots/eks-compute.PNG)
EKS Configuration
![EKS Configuration](./screenshots/eks-compute-nodegroup-configuration.PNG)
4/ Create & Setting Travis for Build source code on Github repository
Travis Configuration
![Travis Configuration](./screenshots/travis-variable-environment.PNG)
Travis Build Successfull
![Travis Build Successfully](./screenshots/travis-build-successfully.PNG)
![Travis Build History](./screenshots/travis-build-history.PNG)
5/ Built Images On Docker Hub
Built Images on Docker Hub
![Built Images Repository](./screenshots/docker-hub-repositories.PNG)
6/ Structure Udagram Application with Microservice Architecture
Microservice Architecture
![Microservice Architecture](./screenshots/microservice-architecture.PNG)
Source File Structure
![Source File Structure](./screenshots/source-file-structure.PNG)
Feed API
![Source File Structure](./screenshots/udagram-api-feed-source-file-structure.PNG)
User API
![Source File Structure](./screenshots/udagram-api-user-source-file-structure.PNG)
FrontEnd
![Source File Structure](./screenshots/udagram-frontend-source-file-structure.PNG)
ReverseProxy Server
![Source File Structure](./screenshots/reverseproxy-source-file-structure.PNG)
7/ Deployment Application with Amazon Elastic Kubernetes Service
Deployment Flow with Elastic Kubernetes Service & Docker
![Build Flow with Docker](./screenshots/docker-image-and-kubernetes-pods.PNG)
Deployment Flow with EKS
![Deployment Flow with EKS](./screenshots/cicd-docker.PNG)
Show Services, Deployments and Pods
![Service, Deployments and Pods](./screenshots/commands-kubectl.PNG)
We can use replicas element for setting the the pod amount on each deployment yaml as `udagram-api-feed.yaml` below:
![Setting Replicas Element](./screenshots/replicas-node-with-ngnix.png)
And we will able to the describe hpa for list out related information
`kubectl autoscale deployment backend-feed --cpu-percent=70 --min=3 --max=5`
`kubectl describe hpa`
![HPA Information](./screenshots/describe-hpa-metrics-server.PNG)

8/ Steps deployment with Elastic Kubernetes Service
Running step by step some command line below
```
kubectl apply -f aws-secret.yaml

kubectl apply -f env-secret.yaml

kubectl apply -f env-configmap.yaml

kubectl apply -f udagram-frontend-deployment.yaml

kubectl apply -f udagram-frontend-service.yaml

kubectl apply -f udagram-api-feed-deployment.yaml

kubectl apply -f udagram-api-feed-service.yaml

kubectl apply -f udagram-api-user-deployment.yaml

kubectl apply -f udagram-api-user-service.yaml

kubectl apply -f udagram-reverseproxy-deployment.yaml

kubectl apply -f udagram-reverseproxy-service.yaml

kubectl apply -f metrics-server-deployment.yaml

kubectl set image deployment frontend frontend=hungdoan2023/udagram-frontend:latest

kubectl rollout  restart deployment frontend

kubectl rollout  restart deployment reverseproxy
```
If we would like to delete pod, service or deployment. Please follow some commands below:
Restart
```
kubectl rollout  restart deployment frontend

kubectl rollout  restart deployment reverseproxy

```
Delete 
```
kubectl delete deployment frontend

kubectl delete deployment reverseproxy
```


9/ Demo Udagram Application
GUI Udagram Application
!['GUI Udagram Application'](./screenshots/demo-udagram-application.PNG)

10/ Debugging, Monitoring and Logging
Log Feed Backend
![Log Feed Backend](./screenshots/log-backend-feed.png)
Log User Backend
![Log User Backend](./screenshots/log-backend-user.png)
Log Frontend
![Log Frontend](./screenshots/log-frontend.png)
Log Reverseproxy
![Log Frontend](./screenshots/log-reverseproxy.png)



