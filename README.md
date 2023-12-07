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

## 8. Demo Project: MongoDB and MongoExpress

![Setup we will make](https://github.com/AmrMekki/KubernetesCourse/assets/59305451/d182e3ad-45d2-479b-bc12-932fb75231ba)
Setup we will make

![Browser Request Flow throught the K8s components](https://github.com/AmrMekki/KubernetesCourse/assets/59305451/2db6638b-ebc0-4f77-86d4-5ae6dddb49ab)

###  Minikube cluster running

#### MongoDB Deployment
create `mongo.yaml` file with configuration through the docker hub documentation

### Secret
create `mongo-secret.yaml` file which holds username and password in base64
`kubectl apply -f mongo-secret.yaml` to have the secret created
`kubectl get secret` shows secret has been created with name **mongodb-secret**

### Connect Mongo to Secret
```
.
.
.
valueFrom:
  secretKeyRef:
    name: mongodb-secret
    key: mongo-root-username
.
. --Same for the password
.
```
Now we run `kubectl apply -f mongo.yaml` so deployment is created
`kubectl get pod --watch` to show the Pod we just created or `kubectl describe` if it takes time

### MongoDB Internal Service
We go back to `mongo.yaml` and we put deployment and service in 1 file because they belong together
run `kubectl apply -f monog.yaml`
> deployment unchanged

run `kubectl get service` to see our created service
run `kubectl describe service [service name]` to find Endpoints on the right ports
run `kubectl get pod -o wide`
run `kubectl get all | grep mongodb` to show all components

### Create MongoExpress Deployment and Service and ConfigMap
create new `mongo-express.yaml` file for mongoexpress service
find in docker hub the mongo express config
and create `mongo-configmap.yaml` and we use it in `mongo-express.yaml` like this
```
.
.
configMapKeyRef:
.
.
```

now we run `kubectl apply -f mongo-configmap.yaml` first
then `kubectl apply -f mongo-express.yaml`
now `kubectl get pod` to see the pod and it is running

### Access MongoDB from a browser

in `mongo-express.yaml` we make an external service for the URL
  - type: "Loadblanacer"
    assigns service an externap IP address and so accepts external requests
  - nodePort: must be between 30000-32767
    Port for external address port
Now we run `kubectl apply -f mongo-express.yaml`
Then `kubectl get service`

`minikube service [service name]` assigns external service an IP address

When we make any changes in URL
Mongo Express External Service --> Mongo Express Pod --> Mongo DB Internal Service --> Mongo DB Pod


## 9. Organizing your components with K8s Namespaces
What is a namespace?
  - We organize resources in namespaces
  - Virtual cluster inside a cluster
  - 4 namespaces per default

### create namespace
`kubectl create namespace my-namespace`
`kubectl get namespace`

### Why use namespaces?

1. if using only default namespace we will have many resources 
ex of grouping:
Namespace:
  - Database
  - Monitoring
  - Elastic Stack
  - Nginx-Ingress
Should not use for smaller projects

2. If we have two teams that use the same cluster
and one team deploys `my-app deployment` that has certain config
if another team deploys accidentally the same name with different config they overwrite the other config
So to avoid this conflict we can use namespaces

3. Resources shring
   re-use components
   blue/green deployment: in the same cluster we can have two different versions of production
   but they use the same resources

4. Limit resources and access to namespaces when working with teams

Characteristics of Namespaces:
  - Each Namespace must define own ConfigMap and Secret
  - Can share services
  - Some components can't be created within a Namespace Like **Volumes and Nodes**

### How to create components in a Namespace
by default, components are in default Namespace
`kubectl apply -f mysql-configmap.yaml --namespace=my-namespace`
or do it in config file then `kubectl get configmap -n my-namespace` which is better documented and more convenient for automatic deployment

### How to change active Namespace
through tool called **Kubens**
```
brew install kubens
kubens
kubens my-namespace --changed name
```

## 10. K8s Ingress eplained

### What is Ingress
External Service VS Ingress
External Service: Through HTTP Protocol (IP address) ex: http://129.1.1.1:8080
Ingress: Domain (my-app.com)

### Ingress YAML Configuration
```
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.com  --What user enters in browser
    http: -- incoming request gets forwarded to internal service
      paths:
      - backend:
          serviceName: myapp-internal-service
          servicePort: 8080 --Forward request to the internal service
```
Host should be
  - valid domain address
  - map domanin name to Node's IP address, which is the entrypoint

We also need an implemntation for Ingress called **Ingress Controller**
Ingress Controller
- evaluates all the rules
- manages redirections
- entrypoing to cluster
- many third party implementations

If we are using Cloud service provider (AWS, Google Cloud, Linode...):
Has out-of-the-box K8 solutions
External Req --> Cloud Load Blanacer --> Ingress Controller (but can be configured in other ways)
> Advantage: dont have to implement load balancer yourself

If using Bare Metal: you need to configure some kind of entrypoint
either inside the cluster or outside as a separate server 

We will use Ingress Controller in Minikube
`minikube addons enable ingress` automatically starts the K8s Nginx implementation of Ingress Controller
Now we can create Ingress rule


### Ingress Controller|

>In minikube cluster
`kubectl get all -n kubernetes-dashboard`

in file `dashboard-ingress.yaml` we setup config
then `kubectl apply -f dashboard-ingress.yaml`
then `kubectl ingress -n kubernetes-dashboard` but address isn't assigned yet it takes time

after we see the address we will map it after editing `sudo vim /etc/hosts`

Ingress default backend
if we run `kubectl describe ingress dashboard-ingress -n kubernetes-dashboard`
we will find a default backend: default-http-backend:80
basically to make error handler if user enters wrong url

### Multiple paths for same host
```
.
.
.
    -host: myapp.com
      http:
      paths:
      - path: /analytics
        .
        .
        .
      - path: /shopping
```
or sometimes subdomains having `analytics.myapp.com` instead of `myapp.com/analytics`
instead we will have multiple hosts each representing a subdomain
```
.
.
.
    - host: analytics.myapp.com
        .
        .
        .
    - host: shopping.myapp.com
        .
        .
        .
```

### Configuring TLS Certificate - https//
```
.
.
.
spec:
  tls:
  - hosts:
    - myapp.com
    secretName: myapp=secret-tls
  .
  .
  .

```
and inside the key file the data keys need to be `tls.crt` and `tls.key`
values are file contents **NOT** file paths/locations
secret component must be in the same namespace as the Ingress component



## 11. Helm Package Manager

### What is Helm
Package Manager for Kubernetes

### What are Helm Charts
A bundle of yaml files so that only one person configure instead of rebuilding the wheel
so we can create and use online Helms

It is also a templating engine
When we have multiple microservices we will have multiple yaml files
But those yaml files all share most of the values
In a template file we can have
1. Define a common blueprint
2. Dynamic values are replaces by placeholders
Template YAML config
```
apiVersion: v1
kind: Pod
metadata:
  name: {{ .Values.name}}
spec:
  containers:
  - name: {{ .Values.container.name}}
  - image: {{ .Values.container.image}}
  - port: {{ .Values.container.port}}
```
values.yaml
```
name: my-app
container:
  name: my-app-container
  image: my-app-image
  port: 9001
```


Another use case
same Applications across different environments
Instead of having a yaml file for each of Development, Staging, and Production
we can have one chart to deploy multiple environments

### Helm Chart Structure
```
mychart/ -- name of chart
  Chart.yaml --meta info about chart
  values.yaml --values for template files (default values that we can override)
  charts/ --chart dependencies
  templates --the actual template files
```
`helm install <chartname>`


### Helm Release Managements

Helm v2 comes in 2 parts
- Client (helm CLI)   --> `helm install <chartname>` to go to server
- Server (Tiller)
Keeps track of all chart executions
- Changes are applied to existing deployment instead of creating a new one
- Handling rollbacks

Downside:
- Tiller has too much power inside of K8s cluster
- Security Issue

in Helm v3 Tiller got removed


## 12. Persisting Data in K8s with Volumes

### Presistent Volume
We need to configure a storage that doesn't depend on the pod lifecycle
Should be available on all nodes
So we have a presistent volume which is a
  - cluster resource
  - created via yaml file
Can be considered like RAM
Needs actual physical storage (local disk or cloud storage)

### Presistent Volume Claim
for a PVC to claim a specific storage through config
- Pod requests volume throuth the PV claim
- Claim tries to find a volume in cluster
- Volume has the actual storage backend

ConfigMap and Secret
- local volumes
- not made by PV 
We can list them in the storage mount

### Storage Class
Provisions Persistent Volumes dynamically when PCV calims it

StorageBackend is defined in the SC component
- via `provisioner` attribute
- each storage backend has own provisioner
- internal provisioner - "jubernetes.io"
- external provisioner
- configure parameters for storage we want to request for PV

It is also claimed by PVC Config

## 13. Deploying Statefu Apps with StatefulSet

### What is StatefulSet
Component used for stateful applications
Like( databases, applications that stores data)
While stateless applications don't keep records and each request is completely new

Stateless Applications
- Deployed using deployment
- replicate your app

Stateful applications
- Deployed using StatefulSet
- replicate Pods

### Deployment vs StatefulSet
- replicating stateful applications is more difficult
- other requirements

If we have MySQL talking to Java
and we need to scale that application
scaling Java is very easy, pods are identical and interchangable, created in random order with random hashes with Load balances

MySQL is more difficult
- can't be created/deleted at the same time
- can't be randomly addressed
- replica pods are not identical
  - Pod requires Pod Identity

Pod Identity:
- sticky identity for each pod
- created from same specification, but not interchangeable
- persistent identifier across any re-scheduling

Why is the identity necessary?
Scaling database appications
1 Pod: for reading and writing
2 or more Pods: one for writing and reading **MASTER** and others only for reading **Worker Nodes**

StatefulSet: fixed ordered names
$(Statefulset name)-$(ordinal)

In statefulSet with 3 replica:
mysql-0 MASTER
mysql-1 SLAVE
mysql-2 SLAVE

Can't create mysql-2 without having mysql-0 and mysql-1 running
Can't delete mysql-1 without deleting mysql-2 first (pop)

So if Pod restarts:
- IP address changes
- name and endpoint stays same

It's complex, Kubernetes help you, but you still ned to do a lot
  - configuring the cloning and data sync
  - make remote storage available
  - managing and backup



## 14. K8s Services

### What is a service and when we need it

Each Pod has its own IP address
- Pods are ephermal - are destroyed frequently
Service
- stable IP address
- loadbalancing
- loose coupling
- within and outside cluster


1. ClusterIP Services
  - default type
Microservice app deployed
And ingress talks to ClusterIP which

2. Service Communication
  - Selector
  - Port
  - Example -->

3. Multi-Port Services

4. Headless Services 


