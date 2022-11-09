# **kubernetes-guide**

## **Installation**

Install a specific k8 version
https://stackoverflow.com/questions/49721708/how-to-install-specific-version-of-kubernetes/50447714

uninstall kubernetes
https://stackoverflow.com/questions/44698283/how-to-completely-uninstall-kubernetes

K8 necessary packages
kubeadm kubernetes administrator. initializes clusters etc.
kubelet  runs pods on different 
kubectl
minikube

## **Components**
---
A kubernetes cluster is composed of a master node or control plane and the worker nodes. The master node is the access to the cluster (and by extension to the k8 virtual network) and is tasked with controlling the rest of the cluster. <br>

NOTE: You can think of a K8 cluster as 2 distribution forms, one physical, one logical.<br>
The physical it's simple, a number of redundant master nodes, and a set of worker nodes.
The logical has a lot of components, Ingress to access the cluster, Pods of which there can be multiple replicas,  and services to link them. there can also be secrets, configurations etc.
A single pod with multiple replicas can be distributed between multiple nodes as k8 sees fit. <br>
multiple pods share a single service interface


## Control Plane (Master Node)
It runs a set of processes that manage the behavior of the cluster and the worker nodes. 

#### *Api Server* 
Entry point to the K8 cluster. all the kubernetes clients will talk to this process.
#### *Controller*
Manager. keeps track of whats happening in the cluster. If something needs to be repaired etc.
#### *Scheduler* 
Ensures pods placement. it knows the available resources of the cluster and the load that a pods needs, then assigns and schedules a pod.
#### *Etcd*
Holds all the configuration data and the status data of each node, and the back up and restore data to recover a node state.
### *Virtual Network. *
Its the component that connects the control plane and all the Worker nodes. Its what makes the cluster work as a single supercomputer.

## Worker Nodes
These components are the machines tasked with doing the work itself. the worker nodes, or simpky nodes, can run one or more pods which are applications themselves.
#### *Ingress*
Its an external entry point to the K8 cluster. like an ip address that can be accessed by anyone outside, and its useful to serve an application to the outside world basically.
#### *Deployment (Pods)*
A deployment is the configuration to run an application. The application (Container) is run within something called a Pod.
A pod is the smallest component in a K8 cluster. they are tasked with running a single container, and each pod gets its own ip address in the K8 virtual network. <br>
When deploying an application, we may want to make multiple instances of it, these are called replicas (Hence why you dont configure a single pod but a deployment which maybe or not be multiple pods). <br>
A deployment of an application allows us to define how many replicas (pods) of an application to distribute among the nodes. K8 checks the resources and knows where to place each replica.
Pods are ephimeral, so they can die if something goes wrong. however, even if a pod dies, its ip address and service will stay the same. 
#### *Service*
A service is the entry point to an application. its a static ip address inside the K8 virtual network and its port. because a single application can be distributed among multiple nodes as multiple pod replicas (Deployment), a service is shared among all of them and its what links them as a single app. since a service is a single entry point to an application, and the app has many copies (replicas), the service knows how to send a request to which replica to distribute the load correctly (its a load balancer).
#### *Config Map*
On a config map the administrator can set some constant values to reference in other configurations files for deployment
#### *Secret*
A secret is similar to a config map but it stores sensible information like passwords. to define variables they need to be stored as base64.
#### *StatefuSet*
