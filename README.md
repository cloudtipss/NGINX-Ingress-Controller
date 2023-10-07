# NGINX-Ingress-Controller
Creating NGINX-Ingress-Controller in your Kubernetes Cluster

## Prerequisites:
1. Kubernetes Cluster: You'll need a running Kubernetes cluster. If you don't have one, follow our [guide](https://cloudtipss.com/Terraform_Eks/) on creating an EKS cluster using Terraform to set up a Kubernetes cluster on AWS Elastic Kubernetes Service (EKS)
2. Kubectl: Ensure that you have kubectl installed on your local machine to interact with your Kubernetes cluster. You can install it following the [instructions](https://kubernetes.io/docs/tasks/tools/) in the official Kubernetes documentation.
3. Helm: Helm is a package manager for Kubernetes that we'll use to install the Nginx Ingress Controller. Install Helm by following the steps outlined in the Helm [documentation](https://helm.sh/docs/intro/install/).

## Connecting to our EKS Cluster

To update kubeconfig file for your cluster use:
```js
aws eks --region <YOUR_CLUSTER_REGION> update-kubeconfig --name <YOUR_CLUSTER_NAME> --profile <YOUR_PROFILE_NAME>
```
Then we can check if we can connect to our cluster:
```js
kubectl get no
```
We would have output with all nodes in our cluster 

## Installing NGINX Ingress Controller

In order to search for available chart versions you can run:
```js
helm search repo ingress-nginx/ingress-nginx --versions
```
For the latest version of Ingress NGINX Helm Chart visit this GitHub [repo](https://github.com/kubernetes/ingress-nginx/blob/main/charts/ingress-nginx/Chart.yaml)

In order to install selected chart run:
```js
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace --version 4.8.1
```

To check if it is installed and running you can run:
```js
kubectl get service -A
```

In your output after a while you should see your Load Balancer address in External Ip field
```js
TYPE           NAME                                 EXTERNAL-IP
ClusterIP      kubernetes                           <none>
LoadBalancer   ingress-nginx-controller            example.elb.amazonaws.com
ClusterIP      ingress-nginx-controller-admission   <none>
ClusterIP      kube-dns                             <none>
                                                                         
```
Copy EXTERNAL-IP Address to your clipboard and open it in a browser. Expected result would be `404 Not Found nginx` error as we don't have any ingresses defined
