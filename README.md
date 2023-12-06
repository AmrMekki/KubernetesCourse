# Kubernetes Crash Course
[Tutorial](https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana)

Thanks to the great **TechWorld with Nana**

## Overview

- Kubernetes

## Course Outline

1. Overview
2. What is K8s
3. MainK8s Components
4. K8s Architecture
5. Minikube and kubectl - Local Setup
6. Main Kubectl Commans - K8s CLI
7. K8s YAML Configuration File
8. Demo Project: MongoDB and MOngoExpress
9. Organizing your components with K8s Namespaces
10. K8s Ingress eplained
11. Helm Package Manager
12. Persisting Data in K8s with Volumes
13. Deploying Statefu Apps with StatefulSet
14. K8s Services



## 1. Overview
Kubernetes Crash Course

## 2. What is K8s
Kubernetes: Open source container orchestration tool
> Developed by Google

Helps you manage containerized applications
  - in different employment environments

What problems does kubernetes solve?
  - having many containers that needs scraping

Good for
  - High Availability
  - Scalability
  - Disaster Recovery


## 3. MainK8s Components

### Server Has

Pod: 
  - Smalles unit of K8s
  - Abstraction over container
  - usually 1 application per Pod
  - Each Pod gets its own IP address (internal)
> We only use kubernetes layers

Service: 
  - Permanent IP address to be attached to each Pod
  - Lifecycle of Pod and Service not connected

Ingress:
  - Services talk to Ingress
  - Ingress is what users get to

ConfigMap:
  - external configuration of your application
> Dont put credentials into ConfigMap

Secret:
  - used to store secret data
  - base64 encoded
> The buil-in security mechanism is not enabled by default
> Use it as environment variables or as a properties file

![Node, Services, Pods, Secret](https://github.com/AmrMekki/KubernetesCourse/assets/59305451/41d13d5d-0d0d-49c2-84ac-827a7d3875f6)


Volumes:
  - storage on local machine / remote outside of the K8s cluster
> K8s doesn't manage data persistance

Replicate everything:
2 Nodes (servers) that connect to the same service
It also has a load balancer

![Server Down](https://github.com/AmrMekki/KubernetesCourse/assets/59305451/04db7690-a469-492d-9eb3-48cfd83aa9b8)

Can't replace database using Deployment (Avoid data inconsistence)
> For stateful apps

Define blueprints for Pods (Deployment)


## 4. K8s Architecture

### Worker Machine in K8s clusetr
- each Node has multiple Pods on it
- 3 processes must be installed on every Node (Kubelet, Kube proxy, Container runtime)
- Worker Nodes do the actual work

Kubelet:
- Interacts with bot the container and node
- Starts the pod with a container inside

Communication between Nodes is done through services

Kube Proxy:
- Forwards the requests

### How to interact with this cluser?
Managing process are done my master processes

4 Process run on every master node
- API Server (Interact through Client)
  - cluster gateway
  - acts as a gatekeeper for authentication
  - some request --> API Server --> Validates request --> other processes --> Pod
  - One entry point in cluster
- Scheduler
  - API Server --> Scheduler (Initiates Pod) --> Where to put the Pod? 
- Controller Manager
  - Detects cluster state changes (like crashing)
  - Controoler Manager --> Scheduler --> Kubelet (to restart Pod) 
- etcd
  - is the cluster brain
  - Cluster changes get stored in key value stores
  - Tells what resources are available
  - Tells the cluster state change
  - Tells if the cluster is healthy
  - Application data is **Not** stored in etcd
 
Master process is made over multiple nodes with load balancers
![Setup of Nodes](https://github.com/AmrMekki/KubernetesCourse/assets/59305451/2c8c7db2-68ce-466c-82a9-830b4e21d394)
Master nodes have less resource usage, if more resources are needed for masters we typically increase number of masters

## 5. Minikube and kubectl - Local Setup

### What is Minikube
Production Cluster Setup (Multiple Master and Worker nodes)
Where to Test?

Minikube is a Node with **Master processes** and **Worker processes** that works on your laptop through a virtual box
So..

#### Minikube:
- creates Virtual Box on your laptop
- Node runs in that Virtual Box
- 1 Node K8s cluster
- for testing purposes 

### What is Kubectl

Master processes communicate through API Server
API Server has 3 clients
  - UI
  - API
  - CLI (KUBECTL) <-- What we will be using

Kubectl is used to deal with any type of cluster

### Installation of Kubectl
> Minikube requires a Hypervisor
- [Minikube Installation](https://minikube.sigs.k8s.io/docs/start/)
- [Kubectl Installation](https://kubernetes.io/docs/tasks/tools/)

After installing try in CLI `kubectl`

Create and Start Cluster
`minikube start --vm-driver=hyperkit`
> Minikube cluster is set up

Run `kubectl get nodes`
> Shows minikube node

## 6. Main Kubectl Commands - K8s CLI

### Commands
Inside Kubectl
1. `kubectl get nodes` Shows all nodes
2. `kubectl get pod` Shows all pods
3. `kubectl get services` Shows all services

Default creates one pod (one replica)
4. `kubectl create deployment nginx-depl --image=nginx` This downloads latest nginx image from docker hub
  - `kubectl get deployment` will show our `nginx-depl`
  - `kubectl get pod` shows a pod now
Layers:
- Deployment manages..
  - ReplicaSet manages..
    - Pod is an abstraction of..
      - Container    
Everything under deployment is managed by kubernetes

5. `kubectl edit deployment [name]` To get auto generated onfiguration file with default values

#### Debugging
6. `kubectl logs [pod name]` Helps with debugging when errors are there
7. `kubectl describe pod [pod name]` If pod isn't running yet
8. `kubectl exec -it [pod name] --bin/bash` Gets terminal of Pod

#### Delete deployment
9. `kubectl delete deplyment [name]`

#### Use configuration file for CRUD
10. `kubectl apply -f [file name]` ex: `nginx-deployment.yaml`
  - `kubectl nginx-deployment.yaml`
  - `vim nginx-deployment.yaml` You will see the basic config for the depl
  - `kubectl apply -f ngin-deployment.yaml` after configuring to confirm
  - If config file doesn't exist we make a new one but if it exists we can edit


## 7. K8s YAML Configuration File
### The 3 parts of configuration files
1. Metadata
2. Specifications
3. Status (Automatically generated and edited by Kubernetes)

### Connecting Deployments to Service to Pods
```
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
DB Service --port80--> nginx Service --port8080--> Pod

`kubectl describe sevice [service name]` to check targetPort and Endpoints

`kubectl get pod -o wide`

To check status
`kubectl get deployment nginx-deployment -o yaml` this shows the yaml file that resides in etcd and will show the status
`kubectl get deployment nginx-deployment -o yaml > nging-deployment-result.yaml` to save and show status

## 8. Demo Project: MongoDB and MOngoExpress

## 9. Organizing your components with K8s Namespaces

## 10. K8s Ingress eplained

## 11. Helm Package Manager

## 12. Persisting Data in K8s with Volumes

## 13. Deploying Statefu Apps with StatefulSet

## 14. K8s Services
