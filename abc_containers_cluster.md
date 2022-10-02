
Cluster: A cluster is the collection of hosts associated with a specific containerized deployment.  


Nodes: Inside of a cluster, there are nodes. A node is the host on which containers reside. 
A host can be either a physical computer or a VM, and it can reside in either an on-prem data center or in a public cloud. 
Generally, there are two categories of nodes in a cluster: the “master nodes” and the “worker nodes”. 
To oversimplify things, a master node is the control plane that provides the central database of the cluster and the API server. 
he worker nodes are the machines running the real application pods.  

Pods: Inside of each node, both Kubernetes and OpenShift create pods. 
Each pod encompasses either one or more container runtimes and is managed by the orchestration system. 
Pods are assigned IP addresses by Kubernetes and OpenShift.  

Container: Inside of pods are where container runtimes reside. 
Containers within a given pod all share the same IP address as that pod, and they communicate with each other over Localhost, using unique ports. 

Namespace: A given application is deployed "horizontally" over multiple nodes in a cluster and defines a logical boundary to allocate resources and permissions. 
Pods (and therefore containers) and services, but also roles, secrets, and many other logical constructs belong to a namespace.
 OpenShift calls this a project, but it is the same concept. Generally speaking, a namespace maps to a specific application,
which is deployed across all of the associated containers within it. A namespace has nothing to do with a network and security construct (different from a Linux IP namespace)  


Service: Since pods can be ephemeral—they can suddenly disappear and later be re-deployed dynamically—a service is a “front end”, which is deployed in front of a set of associated pods and functions like a load-balancer with a VIP that doesn't disappear if a pod disappears. A service is a non-ephemeral logical construct, with its own IP address. 
t