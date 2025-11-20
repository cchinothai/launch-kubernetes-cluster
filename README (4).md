# Deploy Backend with Kubernetes

**Author:** Cody Chinothai  
**Email:** cchinothai@gmail.com

---

## Deploy Backend with Kubernetes

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks4_6cfb382f2)

---

## Introducing Today's Project!

With our docker image pushed and our Kubernetes mainfests setup, we'll finally deploy our backend app. 

### Tools and concepts

This project, Deploy Backend with Kubernetes, focuses on the essential steps to prepare and deploy a containerized application using AWS services and Kubernetes.

Key Concepts: 
- EC2 and EKS Setup: I started by setting up an EC2 instance to interact with Amazon EKS (Elastic Kubernetes Service), which is a managed service for running Kubernetes on AWS.
- Backend Code: I pulled the application's backend code from a GitHub repository.
- Docker and Container Images: I used Docker to build a container image for my backend application based on a Dockerfile. This process packages the code and its dependencies into a consistent unit.
- Amazon ECR (Elastic Container Registry): I created an ECR repository and pushed my Docker container image to it. ECR acts as a secure storage space for my container images, allowing Kubernetes to easily access and deploy them.
- Image Tagging: Tagged Docker image (e.g., with "latest") to manage different versions in ECR and identify correct image. 

### Project reflection

// time ~ 3 hours totla

---

## Project Set Up

### Kubernetes cluster

To set up today's project, I launched a Kubernetes cluster. The cluster's role in this deployment is to be the command center that tells us when our ec2 instance nodes should start/stop and scale containers and keep them connected to the internet. 

### Backend code

download git in your ec2 instance terminal and use git clone with the backend repo 

### Container image

Once I cloned the backend code, I built a container image because it allows Kubernetes to setup identical instances that all the same no matter the environment. Without an image, it would be difficult for Kubernetes to deploy and orchestrate your app since it won't have the right installation/run instructions. 

I pushed my container image to Amazon ECR because container registries like ECR act as storage spaces for your container images. This is essential because Kubernetes needs a place to pull these images from when it deploys your application.

ECR is an excellent choice for this project as it's an AWS service, which allows Elastic Kubernetes Service (EKS) to easily find and deploy your container image with minimal authentication setup. This workflow also helps ensure consistency across different environments and enables Kubernetes to quickly scale your application by spinning up identical containers.

---

## Manifest files

A Kubernetes manifest acts as a set of instructions that tells Kubernetes how to run your app. Kubernetes uses it to know what your app needs, like which containers to run, how many copies to create, and how much memory to allocate.

Without manifests, Kubernetes wouldn’t know how to set up and manage your app automatically. You'd have to manually configure each container every time you deploy, which would be confusing, error-prone, and hard to repeat. Manifests make deployment much more simple and consistent.

When you deploy an app with Kubernetes, Kubernetes' job is to manage several copies of your app across multiple containers in your cluster. It does this using a Deployment resource, which is a piece of software inside your cluster. The Deployment resource handles creating and replacing groups of containers, scaling your app, and rolling out updates.

A Deployment manifest tells Kubernetes exactly how to manage these tasks. It includes details like the number of copies of your code that Kubernetes should run across your cluster, and which settings to apply (e.g. CPU limits, container image, or network settings).

The Kubernetes service lets you access your application using your node's IP address and port number. Other ways to expose your app include using a load balancer (external IP) or Ingress (advanced traffic routing). 

We use NodePort for testing/learning how services work. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks4_b01876554)

---

## Backend Deployment!

To deploy my backend application, I installed kubectl and applied our deployment and service manifests

### kubectl

kubectl is a command-line tool for interacting specifically with Kubernetes resources once our cluster is up and running. 

I need this tool to to apply our manifests and deploy our application

I can't use eksctl for the job because while it was used to set our EKS cluster, it shouldn't be used to deploy applications and manage resources within the cluster. 

First install kubectl: 
$ sudo curl -o /usr/local/bin/kubectl \
https://s3.us-west-2.amazonaws.com/amazon-eks/1.31.0/2024-09-12/bin/linux/amd64/kubectl

Then grant permission to use kubectl: 
$ sudo chmod +x /usr/local/bin/kubectl


![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks4_6cfb382f2)

---

## Verifying Deployment

My extension for this project is to use the EKS console to verify our cluster nodes to spec.  I had to set up IAM access policies because even though we had Admin priviledges, Kubernetes has it's own access management rules. 

 I set up access by creating an identity mapping in the terminal between our cluster and IAM user: 

$ eksctl create iamidentitymapping --cluster nextwork-eks-cluster --arn your-user-arn --group system:masters --username admin --region your-region-code


Once I gained access into my cluster's nodes, I discovered pods running inside each node. Pods are the smallest deployable units in Kubernetes. Containers in a pod share the same network and storage space so they can communicate and share data efficiently. 

The EKS console shows you the events for each pod, where I could see the history from when the container was create to when it was assigned. This validated the following steps: 

Successfully assigned default/demo-flask-backend...: Your pod has been given an internal IP address in the cluster.

Pulling image...: The pod is getting your container image from Amazon ECR.

Successfully pulled image...: The image was successfully downloaded to the cluster.

Created container demo-flask-backend: The container was built from your image and prepared for launch.

Started container demo-flask-backend: The container has been successfully started and is now running in your pod.

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks4_3b391f873)

---

---
