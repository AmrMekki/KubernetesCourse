# Kubernetes Crash Course
[Tutorial](https://www.youtube.com/watch?v=X48VuDVv0do&ab_channel=TechWorldwithNana)

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

Volumes:
  - storage on local machine / remote outside of the K8s cluster
> K8s doesn't manage data persistance

Replicate everything:
2 Nodes (servers) that connect to the same service
It also has a load balancer
Can't replace database using Deployment (Avoid data inconsistence)
> For stateful apps

Define blueprints for Pods (Deployment)


## 4. K8s Architecture

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
