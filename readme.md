
# CICD Project with ArgoCD ( Pull Based Deployment )

This project is all about CICD pipeline with ArgoCD . We are going to create CICD project by simply hosting spring boot application on AWS EKS cluster . we will be following gitOps principle for our project where i have created 2 git repository  i.e, one for application code & another for kubernetes manifests files . 




## Git Repository For application code available at below link:

https://github.com/akashzakde/spring-boot-app.git

## Git Repository For K8S manifests code available at below link :

https://github.com/akashzakde/k8s-manifests-repo.git

Both git repository are public .

## Tools and technology used in our project are : 

-  Jenkins
-  Git
-  MAven
-  SonarQube Scanner & Server
-  Docker & Docker Hub Registry
-  ArgoCD
-  AWS EKS Cluster
## Below is our CICD Project Diagram :

<img width="647" alt="CICD Project With ArgoCD diagram " src="https://github.com/akashzakde/spring-boot-app/assets/64258131/0dfa0ae0-7b5f-470a-b2cf-10821ed5bbb5">

## Installation Steps :

Step 1 : Login to AWS management console using our email id & password

Step 2 : Create Security Group For jenkins & sonarqube server , Allow port 22 , 8080 in jenkins security group & allow 22 , 9000 in sonarqube security group .

Step 3 : Create Key Pair 

Step 4 : Launch two t2.medium EC2 instances (one for jenkins & another for sonarqube server).

Step 5 : Login to jenkins server using public ip address & setup jenkins server .

Step 6 : Login to sonarqube server using public ip address & setup sonarqube server .

Step 7 : Now create AWS EKS cluster , you can create EKS cluster using terraform by using this github project https://github.com/akashzakde/eks-cluster-using-terraform.git

Step 8 : Login to jenkins dashbaord using url http://{jenkins-server-ip}:8080 

Step 9 : Install Maven Integration, Github Integration, Sonarqube Scanner plugins on jenkins .

Step 10 : We need github token for pushing changes to github repo & docker hub credentials for pushing docker images to docker hub registry . please add these credentials in jenkins . 

Step 10 : We need to add sonarqube server details & its credentials , add tools like maven , git & java jdk path inside jenkins.

Step 11 : Now login to sonarqube server using url http://{sonarqube-server-ip}:9000 , create new project for java , create webhook for jenkins . 

Step 12 : Login jenkins dashboard create 2 pipelines , One pipeline for Continuous Integration & another pipeline for Continous Delivery . Continous integration project is at https://github.com/akashzakde/spring-boot-app.git & Continous Delivery project is at https://github.com/akashzakde/k8s-manifests-repo.git

Step 13 : Inorder to run our CICD pipeline automatically on new commit , we need to add webhook on our CI git repo i.e. https://github.com/akashzakde/spring-boot-app.git 

Step 14 : Finally we need to install ArgoCD on EKS cluster & create new application with https://github.com/akashzakde/k8s-manifests-repo.git as a CD git repository .
