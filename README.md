# EKS-Workshop

This is a simple web application using Python Flask. This is used in the demonstration deployment of sample application using Kubernetes

Below are the steps required to get this working on a base linux system

- Installing Kubectl 
- Create EKS cluster in AWS
- Create node-group in EKS cluster
- Deploying aws-auth configmap
- Check the nodes are attached to the cluster
- Deploying nginx ingress controller 
- Deploying sample app
- Attaching ELB to auto scalling group
