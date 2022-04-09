# How to deploy an application on IBM Cloud and expose it to public, from scratch
## Introduction
This tutorial is for you if you:
- want to deploy an application on IBM Cloud
- want a public url for your application to be visited by anyone
- is not a professional in deploying and maintaining applications
- is confused and overwhelmed by a list of official documents found on IBM Cloud

The author of this article had wasted huge amount of time and energy on figuring out how to deploy an application on IBM Cloud.   
He wrote this article, wishing to help those who are new to this sophisticated cloud service and simply want to get their applications online.

## Before you start, check that you have meet the following requirements:
### Mandatory:
- Have an IBM Cloud account, and have access to basic free services.
- Have installed IBM CLI on your computer. Link to how to install: 
https://cloud.ibm.com/docs/cli?topic=cli-getting-started
- Have installed Docker on your computer. Link to Docker:
https://docs.docker.com/get-docker/
### Recommended:
- Have basic understanding on Docker commands. Docker is a commonly used tool in building and deploying applications. Link: https://docs.docker.com/get-started/
- Have basic understanding on what is Kubernetes and how it works (No need to know how to write commands!). Understanding how it works would be helpful in extending your applications and adding more services. Link:  https://kubernetes.io/docs/tutorials/kubernetes-basics/

## The main tutorial
### Step 1: Get your executable file ready
The author of this article builds his application using Java and Spring Boot, but it doesn't matter what language or frameworks you are using.   
For java applications using maven, after running mvn package, you should have a jar file in the target folder.   
For other applications, ensure the executable file is ready.
### Step 2: Add a Docker file in your working directory
Add a Dockerfile to ensure your executable file will be added to the Docker image later created.   
The file name should be Dockerfile, with no extensions.   
The below is the sample Dockerfile for author's Spring Boot application.
```Dockerfile
FROM openjdk:8-oracle
COPY target/*.jar app.jar
CMD ["--server.port=8080"]
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```
The FROM means the Docker image to be created relies on the JDK image. As is known to Java programmers, a jar can only be run on JVMs.   
The COPY means the jar file in the target folder shall be put into the Docker image we create later.   
The CMD and EXPOSE means the application shall expose the port 8080 to be visited from outside.   
The ENTRYPOINT menas the application shall start by executing the jar file.   

Browse the Internet if you are unsure about how to write Docker file for your own application.
## Step 3: Build your application into an image
Decide a name for the image and run the following command. Don't miss the dot!
```
docker build -t <the name of your image> .
```
## Step 4: Create a resource group on your IBM Cloud account.
To start managing your resource groups, in the IBM Cloud® console, go to Manage > Account > Account resources > Resource groups. You can create, view, and rename your resource groups, add resources and manage access to your resource groups.
![image](https://user-images.githubusercontent.com/55789171/162568009-4056d8b1-ed08-4a25-a18f-b06e8fa46451.png)
## Step 5: Create a cluster for Kubernetes.
Click on the upper left button in your cloud console, then click on Kubernetes, then click on clusters, then click on create new cluster.   
You can create a free cluster that expires in 30 days, ask your client for a standrad cluster or you can simply create another one 30 days later.   
Make a note of the cluster name as it shall be needed later.
It can take sometime for the cluster to be created, so go take a rest at this moment ;)   
![2022-04-09 (3)](https://user-images.githubusercontent.com/55789171/162568342-6771cac4-689a-4ee4-9754-9c7f8b22ed48.png)
## Step 5: Create a namespace 
Click on the upper left button in your cloud console, then click on Kubernetes, then click on Registry.
![2022-04-09 (2)](https://user-images.githubusercontent.com/55789171/162568174-602bdaa2-f094-4f8a-a79e-358c7ea241e3.png)
You should be presented with the following page.   
Click on the Namespaces and create your own name space.   
Make a note of the namespace name as it shall be needed later.
![1649500915(1)](https://user-images.githubusercontent.com/55789171/162568498-68543c37-20b5-4f43-af28-5f3bc60e82b7.png)
## Step 6: Tag your image you created in step 3
Run the following command. You already have the name of your image and your namespace.   
Replace the "a name of your repository" with the name you like, for your repository, which can be viewed later.   
```
docker tag <the name of your image> uk.icr.io/<the name of your namespace>/<a name of your repository>
```
