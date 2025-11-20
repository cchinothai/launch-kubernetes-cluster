# Set Up Kubernetes Deployment

**Author:** Cody Chinothai  
**Email:** cchinothai@gmail.com

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

In this project, we will prepare a backend app to launch with Kubernetes using Docker and and Elastic Container Registry (ECR)

### Tools and concepts

- Launched an EC2 instance and set it up with eksctl to create an EKS cluster
- clone a backend app from Github to build and push a corresponding Docker image
- pushed my Docker image to an ECR repo

### Project reflection

This section took me approximately 90 minutes. 

Something new that I learnt from this experience was exactly how to create an ECR repository from EC2 instance connect and push our docker image into the registry. 

---

## What I'm deploying

Launch a kubernetes cluster with the following command: 

$ eksctl create cluster \
  --name nextwork-eks-cluster \
  --nodegroup-name nextwork-nodegroup \
  --node-type t3.micro \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 3 \
  --version 1.33 \
  --region your-region-code


### I'm deploying an app's backend

Use the this command: 
sudo dnf update
sudo dnf install git -y

Then configure your credentials: 

$git config --global user.name "yourname"
$git config --global user.email "email"



![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step is to build a container image of the backend. This is so Kubernetes can create identical containers to use in deployment, all of which run the same container image for reproducability. 

trying this command: 
$ docker build -t nextwork-flask-backend .
- we realize that we need to install Docker first: 
$ sudo yum install -y docker
- then we need to start docker: 
$ sudo service docker start






When I tried to build a Docker image of the backend, I ran into a permissions error because we didn't use sudo to run docker commands with the root account. Instead of doing this with sudo, it's better practice to give the respective commands with our current ec2-user. 

our current user: 
$whoami // returns ec2-user

grant permissions (add user to the docker group): 
$ sudo usermod -a -G docker ec2-user


To solve the permissions error, I added our current ec2-user to the docker group. That way we can avoid running docker commands with sudo if we aren't originally given those permissions in a different setting. 

You can check your current user's allowed groups with 
$ groups <ec2 username>

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I'm using Amazon ECR in this project to store the docker images that I've built. This is so that Kubernetes can pull updated images directly from this registry. 

Container registries like Amazon ECR are great for Kubernetes deployment because they keep your Kubernetes nodes updated. Otherwiise, each time your docker image changes, you need to manually pull each image to get new updates. 

![Image](http://learn.nextwork.org/enthusiastic_turquoise_radiant_monstera_deliciosa/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app's backend code, I learned how our container image preserves the functionality of our code by specifying the necessary requirements used in our flask backend that uses a search API, along with the right python and linux version to run it with. This ensures that the code will run identically from local to. any other location. 

### Unpacking three key backend files

The requirements.txt file lists the dependencies need for the backend: 

Flask==2.1.3: the web framework used to build the backend code, which you'll see in app.py.

flask-restx==0.5.1: creates an API using an extension Flask-RESTx so that users or other applications can make requests to our backend.

requests==2.28.1: The Requests library is used to get data from the Hacker News API.

werkzeug==2.1.2: Werkzeug helps Flask handle application-level routing. For example, when a user/service makes a request to our backend, Flask uses its routing system (powered by Werkzeug) to direct that request to the right function in app.py, which then processes the request and responds to it.

The Dockerfile gives Docker instructions on how to build the image: 

uses python 3.9 on Alpine Linux, which is a lightweight version of Linux since our app size is small (making it faster to build and run). 

LABEL adds author metadata to the image

WORKDIR /app - sets /app as the working directory in the container, so the other commands in the Dockerfile run from there. 

It will install all the necessary parts within requirements.txt by copying it from our local machine and using it as a parameter for pip3 install

COPY . . - copies files from the current directory into /app directory

CMD - instructs the container to run $python3 app.py to start our flask app.

The app.py file uses Flask to create an API that uses a route to direct incoming elements to the SearchContents class. It fetches this data by sending a get() request to the API and collects data to format into a JSON response. 

---

---
