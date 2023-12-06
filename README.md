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

## 6. Main Kubectl Commans - K8s CLI

## 7. K8s YAML Configuration File

## 8. Demo Project: MongoDB and MOngoExpress

## 9. Organizing your components with K8s Namespaces

## 10. K8s Ingress eplained

## 11. Helm Package Manager

## 12. Persisting Data in K8s with Volumes

## 13. Deploying Statefu Apps with StatefulSet

## 14. K8s Services
