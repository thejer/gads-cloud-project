# Lab: Google Cloud Fundamentals: Getting Started with GKE

## Overview:

In this lab, you create a Google Kubernetes Engine cluster containing several containers, each containing a web server. You place a load balancer in front of the cluster and view its contents.
             
## Objectives: 

In this lab, you will learn how to perform the following tasks:
- Provision a Kubernetes cluster using Kubernetes Engine.
- Deploy and manage Docker containers using `kubectl`.

## Task 1: Confirm that needed APIs are enabled

### Steps: 
1 Enter the following to display the project IDs for your Google Cloud projects:  
```
gcloud projects list
```
2 Using the applicable project ID from the previous step, set the default project to the one in which you want to enable the API:
```$xslt
gcloud config set project YOUR_PROJECT_ID
```
3 Get a list of services that you can enable in your project:
```$xslt
gcloud services list --available
```
If you don't see the API listed, that means you haven't been granted access to enable the API.

4 Using the applicable service name from the previous step, enable the services:
```$xslt
gcloud services enable Kubernetes Engine API
```
```$xslt
gcloud services enable Container Registry API
```

## Task 2: Start a Kubernetes Engine cluster

### Steps:
1 In GCP console, on the top right toolbar, click the Open Cloud Shell button and press continue.

2 For convenience, place the zone assigned to you into an environment variable called `MY_ZONE`. 
At the Cloud Shell prompt, using the following command:

```$xslt
export MY_ZONE=us-central1-a
```
3 Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster `webfrontend` and configure it to run 2 nodes:

```$xslt
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```
It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

4 After the cluster is created, check your installed version of Kubernetes using the `kubectl version` command:

```$xslt
kubectl version
```
The `gcloud container clusters create` command automatically authenticated `kubectl` for you.

## Task 3: Run and deploy a container

### Steps:

1 From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)
```$xslt
kubectl create deploy nginx --image=nginx:1.17.10
```
In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

2 View the pod running the nginx container:
```$xslt
kubectl get pods
```

3 Expose the nginx container to the Internet:
```$xslt
kubectl expose deployment nginx --port 80 --type LoadBalancer
```
Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

4 View the new service:
```$xslt
kubectl get services
```

You can use the displayed external IP address to test and contact the nginx container remotely.
It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the `kubectl get services` command every few seconds until the field is populated.


5 Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

6 Scale up the number of pods running on your service:
```$xslt
kubectl scale deployment nginx --replicas 3
```
Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

7 Confirm that Kubernetes has updated the number of pods:
```$xslt
kubectl get pods
```

8 Confirm that your external IP address has not changed:
```$xslt
kubectl get services
```

9 Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.
