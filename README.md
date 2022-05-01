# EKS-Workshop

This is a simple web application using Python Flask. This is used in the demonstration deployment of sample application using Kubernetes

Below are the steps required to get this working on a base linux system

- Install Kubectl 
- Create IAM role
- Create EKS cluster in AWS
- Create node-group in EKS cluster
- Deploying aws-auth configmap
- Check the nodes are attached to the cluster
- Deploying nginx ingress controller 
- Deploying sample app
- Access the app in browser
- Test daemonset
- Attaching ELB to auto scalling group


Please clone this repo, before proceeding. `git clone https://github.com/anil4u-04/EKS-workshop.git`

## 1. Install Kubectl

Run the below commands in terminal

      curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

Move the downloaded file to $PATH

    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

Check the installation

    kubectl version --client
    
## 2. Create IAM role

We will use AWS IAM console to create the roles and attach the permission in [IAM](https://github.com/anil4u-04/EKS-workshop/tree/main/iam)folder.

## 3. Create EKS cluster in AWS.

We are going to use AWS console to create AWS EKS cluster.

## 4. Create node-group in EKS cluster

This one also we will AWS console to create node groups.

## 5. Deploying aws-auth configmap

Before running below command please replace `arn:aws:iam::<aws account-id>:role/<role name>` this your role ARN.

    kubectl apply -f ./aws-auth-configmap.yaml 
    
## 6. Check the nodes in cluster

after successfully deployed configmap , wait for couple of seconds and run below command to check whether the nodes are attached to the cluster

    kubectl get nodes

Now you can see nodes attached to the cluster. 

## 7. Deploy nginx ingress controller

Run the below command to deploy nginx in K8s cluster

    kubectl apply -f nginx-ingress.yaml

The above command will install below nginx deployment and also service type LoadBalancer, which will automatically create a ELB in AWS.
Please login to AWS console go to EC2 --> Loadbalcners, you can see a load balancer is up and running

## 8. Deploy sample app in EKS cluster

Run below commands to deploy sampleApp in K8s cluster

    kubectl apply -f deployment.yaml

check whether the deployment is deployed and the pods are up and running

    kubectl get pods

you can see something like this

    sampleapp-deployment-77d858b977-sbsk7                  1/1     Running     0          45s
    sampleapp-deployment-77d858b977-wvlsj                  1/1     Running     0          45s
    sampleapp-deployment-77d858b977-xksxp                  1/1     Running     0          45s

which indicates our app is UP and running

Now deploy service, for the same please run below command

    kubectl apply -f service.yaml

Now check the status of the service.

    kubectl get svc

you can see something like this

    sampleapp-deployment-service          ClusterIP   10.100.61.209    <none>        5000/TCP                     33s

That indicates, service is UP and running.

## 9. Access the app in the browser.

Since we don't have any DNS entries , we will override the host entries in /etc/hosts file , steps to do so

 1. Get the IP of ELB `nslookup <ELB DNS name>`
 2. Open /etc/hosts and add this line at the end of the file `<one of the IP from above command> sampleapp.mysite.com` save and close the file
 3. Open browser and access http://sampleapp.mysite.com , you can see the O/P of our app.

## 10. Test daemonset

Since we deployed our app as deployment , now you can test the daemonset. Please run the below command to deploy app as daemonset.

      kubectl apply -f daemonSet.yaml

Now you can see the pod is running in all the nodes. 

      






 
