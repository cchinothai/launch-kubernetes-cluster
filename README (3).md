# Create Kubernetes Manifests


**Author:** Cody Chinothai  
**Email:** cchinothai@gmail.com

---

## Create Kubernetes Manifests

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks3_b01876555)

---

## Introducing Today's Project!

In this project, I will 

🛫 Set up a Deployment manifest that tells Kubernetes how to deploy your backend.

🚚 Set up a Service manifest that tells Kubernetes how to expose your backend to your users.

Dive into the details of your manifest files and discover new learnings!



### Tools and concepts

Key Learnings
- Built and pushed a container image into Amazon ECR
- Deployed manifests to instruct Kubernetes on how to manage and scale app
- Write service manifests to expose and handle traffic
- Used Amazon EKS to launch EC2 into a Kubernetes cluster

### Project reflection

Continue to deploy using Kubernetes

This part took me approximately 90 minutes

---

## Project Set Up

### Kubernetes cluster

// Launch Kubernetes Cluster

### Backend code

// Pull backend from github repo 

### Container image

//Build container image with docker from ec2 instance connect

//Push container image into ECR

---

## Manifest files

Manifests tell Kubernetes how to run your app by specifying what containers to run, how many copies to create, and how much memory to allocate. 
- Tells Kubernetes how to setup your app automatically. 

A Kubernetes deployment manages how to get the app running and keeping those deploymant instances healthy.  The container image URL in my Deployment manifest tells Kubernetes the docker image that we pushed to ECR. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks3_b01876554)

---

## Service Manifest

A Kubernetes Service exposes traffic to your Kubernetes node. You configure how to create or update the traffic controller and list which pods to traffic. You need a Service manifest to specify what kind of traffic to accept and what ports to listen on. 

To allow users outside the cluster to reach out app without knowing its internal details: 
- we send external traffic to port 8080. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks3_b01876555)

---

## Deployment Manifest

Annotating the Deployment manifest helped me understand the levels at which the Kubernetes API runs configurations for our entire deployment and for specific nodes while also setting up traffic rules.

Replicas are the amount of identical instances we make for the app, which we refer to these instances as pods. Pods are the smallest unit that make up a Kubernetes cluster. 

note that everything under template specifies configuration at the pod level. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks3_6aae73e71)

---

---
