# Launch a Kubernetes Cluster

**Author:** Cody Chinothai  
**Email:** cchinothai@gmail.com

---

## Backend Repo: https://github.com/cchinothai/nextwork-flask-backend

<img width="2552" height="1386" alt="image" src="https://github.com/user-attachments/assets/17ca4218-c814-4860-8da3-870c85be4129" />

<img width="1627" height="781" alt="image" src="https://github.com/user-attachments/assets/ab35d4f3-ebd3-42bf-933a-1fe53e88e557" />

<img width="1511" height="790" alt="image" src="https://github.com/user-attachments/assets/eaf63d2e-7376-48c1-83e9-a899827a2943" />


## Launch a Kubernetes Cluster

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks1_e5f6g7h8)

---

## Introducing Today's Project!

Key Learnings
1. Launch and connect to an EC2 instance.
2. Create a Kubernetes cluster.
3.  Monitor cluster creation with CloudFormation.
4.  Access your cluster using an IAM access entry.
5.  Test the resilience of our Kubernetes cluster.

### What is Amazon EKS?

### One thing I didn't expect

### This project took me...

---

## What is Kubernetes?

Kubernetes is a container orchestration platform that we can use to ensure our containerized applications are running and can dynamically scale based on traffic. 

In this command: 
$eksctl create cluster \
--name nextwork-eks-cluster \
--nodegroup-name nextwork-nodegroup \
--node-type t3.micro \
--nodes 3 \
--nodes-min 1 \
--nodes-max 3 \
--version 1.33 \
--region us-east-2

We specify a node range of 1 through 3 to provide basic scaling for experimental purposes. 

I used eksctl to define a new cluster and a node group with each node running on t3.micro instances and using Kubernetes version 1.31

I initially ran into two errors while using eksctl. The first one was because we didn't have eksctl properly installed, and the other because we didn't set proper permissions for ec2 to execute our commands. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks1_ff9bfc221)

---

## eksctl and CloudFormation

CloudFormation helped create my EKS cluster because it generates the infrastructure as code specifying the resources needed to create the cluster,  including VPCs, security groups, route tables, etc. It created new VPC resources because it requires settings specific to creating a kubernetes cluster. 

There was also a second CloudFormation stack for the node groups. The difference between a cluster and node groups is:
 - the cluster represents the entier kubernetes environment
- the node groups are a component of your cluster that are grouped together to manage instance type and resource limits. 

It is better design to have separate independent stacks for the cluster and node group so that you can isolate issues better when troubleshooting, especially if one half fials 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks1_w3e4r5t6)

---

## The EKS console

I had to create an IAM access entry in order to map our IAM role to Kubernete's RBAC role. An access entry is like a handshake protocol that connects AWS and Kubernets.  I set it up by creating and access entry point linking our user IAM role and attaching the AmazonEKSClusterAdminPolicy

It took. ~30 minutes to create my cluster. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks1_e5f6g7h8)

---

## EXTRA: Deleting nodes

---

---
