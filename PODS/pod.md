Kubernetes Pods
-
- Kubernetes is preferred over docker as K8S is a cluster, it offers auto scaling, healing and enterprise support which we can use for our containers
- When we are moving from Docker to kubernetes, lowest level of deployment in K8s is pod. In K8s we dont directly deploy container like docker. K8S dont deploy apps and containers, instead deploy them as pod. Pod can be single container/multi container
- The end goal in docker or kubernetes, we have to deploy app in containers known as containerization

- Pod is defined as how to run a container
- In docker to run container we provide many arguments on CLI like -d -p -v. Whereas in K8S we pass those specifications in pod.yml file
- So IN K8S we've wrapper but it extracts commands from pod.yml

- In K8S instead of containers, we deploy a pod which can be single container or multiple. 
- If we've application deployed inside container and it has to read some files from another container, so instead of creating different pods we can put these containers in single pod. This way both containers will have some advantages
  - Grouping containers in single pod allows shared networking, shared storage. Containers can talk to each other using localhost. Apps are directly accessed and info can be retrieved. They can share files between them as well.
 - Container will be inside pod. K8s allocates cluster IP address to POD and we can access apps inside containers using POD cluster's IP address. Kube proxy generates this IPS address.

 - **POD** :- Wrapper that K8s has created for container to make life of devops engineer easier. It defines everything in yml file. Most of the times pod have single container which is accessed by cluster IP address. Kubproxy generates IP address

 - **Kubectl** :- Just like docker CLI for K8S, CLI for K8S to interact with K8S cluster. To create, modify, delete resources

 - **Minikube** :- Command line tool which allows to create k8s cluster

Practical Demo
-
- Install kubectl in windows
- Check version :- **kubectl version**
- To setup local K8S cluster (minikube)
  - Install Minikube
 
- To create minikube cluster :- **minikube start**
  - On windows minikube creates VM first and on top of it single node K8S clsuter is setup. (In prod there is multi-node K8S cluster). For this we need virtualization platform, use below command
  - Command :- **minikube start --memory=4096 --driver=hyperkit**
  - If we just do "minikube start", our K8S cluster will use default docker driver which is not recommended while moving into K8S containerization
 
  ![image](https://github.com/user-attachments/assets/2d1a9295-0804-4834-9473-581316d5abcc)

- To check kubectl is connected to K8S cluster :- kubectl get nodes
  - Here the name of node is "minikube" with "ready" status and that node is both data and control plane for us
 
  ![image](https://github.com/user-attachments/assets/ca0ae248-64fa-4e4a-b31e-afe4ee96ecbd)

  
  
