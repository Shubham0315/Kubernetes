# Kubernetes
The project elaborates all the aspects of K8S including its architecture with data and control planes. It explains K8S concepts like pods, deployments, services, replicas, config maps, ingress, etc.


Difference between Docker and K8S
-
- Docker is a container platform. Docker makes interaction with containers. Docker makes container journey very easy as it provides complete container lifecycle whether using docker engine or docker CLI
- Kubernetes is container orchestration platform

Problems with Docker
-
- **Single Host nature of Docker**
  - Containers are ephemeral(short life) in nature. Means they can die and revive anytime.
  - If we have multiple containers on top of docker(Single host) and one of the containers takes more memory of the host, then any other container doesn't get enough resources so it will die or it wont get started. So here if container is short of memory resources or image is not getting pulled, the container will die.
  - We face this problem due to "SINGLE HOST" on top of which we've multiple containers installed and running
  - Here one container impacts another
 
- **Auto Healing**
  - If someone kills our container, application inside it wont be accessible unless someone start the container/act upon the container. 
  - This has to be done without user's manual intervention, container should start by itself. This process is called "AUTO HEALING".
  - This doesn't happen with docker as there is no Auto Healing capability in docker
  - In organization we can have multiple containers whose monitoring cannot be done manually by devops engineer. So there has to be mechanism of "Auto Healing" just in case any container goes down to avoid traffic issues on application

- **Auto Scaling**
  - We have physical host/EC2 on top of which we've installed docker and one container on top of it
  - Our host has 4CPU and 4 Memory. But if our application is loaded beyond the threshold capacity, to act upon increasing load we need "AUTO SCALING". As soon as container sees application is over loading (traffic increasing), it should scale up itself(or its instances) automatically. This can also be done manually increasing containers from 1 to 10 for increased traffic.
  - Docker doesn't support both auto or manual scaling of resources.
  - Auto scaling is achieved by Load Balancer so that we can scale resources and distribute the load/traffic as required
  - e.g:- In netflix if we've 20k users, using LB we can distribute users among 2 IPs so our site doesnt get much load and users can surf seamlessly

- **Minimalistic Docker**
  - Docker is very minimalistic/simple platform as it doesn't support any Enterprise level applications
  - For enterprise apps we need LB, Firewalls, auto scaling, auto healing, api gateways to run which docker doesn't support making it minimalistic platform

All these problems are solved by Kubernetes
-
- **Multi Node** 
  - By default Kubernetes is a cluster (Group of nodes)
  - Docker is installed on single machine but K8S is installed on master-node architecture where we create clusters like one master node and multiple worker nodes
  - If any of container is dying, K8S will just put the container in different node. As K8S has multi node architecture, it puts apps/pod in diff node.
 
- **Auto Scaling**
  - K8S have replica sets. If we've application with increasing load, in yml file we can increase replicas of our application from 1 to n dur to increasing traffic (manual)
  - We can also automate this thing using Horizontal Pod Autoscalar. We can set condition in yml like if threshold for traffic is breached, just launch more container.
 
- **Auto Healing**
  - Whenever there is a chance of damage to container, kubernetes controls damage and fix it.
  - Suppose our container goes down. Docker will restart or recreate container. Whereas due to kubernetes auto healing feature, even before our container goes down, kubernetes will start/rollout new container.
  - Due to this end user will be able to access the application without having latency issues
 
- **Enterprise Support**
  - Kubernetes is Enterprise level container orchestration platform
  - Thats why docker individually is never used in prod as it doesnt support Enterprise apps although docker swarm can be used
  - K8S provides enterprise solutions to our applications
 
K8S doesnt support advanvced Load Balancing capabilities

Kubernetes Architecture
- 
- Single node means single server, multi node means multile servers. Multi-node is cluster
- So in K8S we manage cluster not single node where master and worker nodes exist
- There are some services on master to get the work done by control node
- Our application work on worker node on K8S cluster, none of the container work on master node
- 
- In docker simplest thing is container whereas in K8S its pod
- Suppose we've VM on top of which docker is installed and container is running on it. If we run container or install (java) application and we dont have its runtime (java) our application won't run. Similarly while running container we need container runtime. In docker we have container runtime component known as "DOCKER SHIM"
- But in K8S as it provides enterprise support with auto healing, auto scaling, we create master and worker node here (multiple worker can be there). So in K8S we dont directly send request to worker, it has to go through master. Means request goes from control plane to data plane.
- In K8s, pod is smallest unit deployable which gets deployed on worker node. When user tries to deploy pod, "KUBELET" is responsible for deployment/running of pod and its maintenance (pod is always running). Even for running pod we need container runtime but here docker is not mandatory. Container runtime in K8s can be dockershim, container D, creo to implement K8S interface. Kubelet ensures our pod is always running, if pod is not running kubelet notifies K8S of the same.
- In docker there is default networking "Bridge" which is mandatory to run pod. Even in K8s, we have kubeproxy which provides us networking as every container/pod must be allocated with IP address and LB capabilities. If we scale our pod in no of replicas, LB is used by kubeproxy. So request handling can be done by kubeproxy

- _**Data Plane / Worker Node**_
  - Worker node is basically required to run our application using below 3 components
  - 3 components are there :- kubeproxy, kubelet, container runtime
  - **Kubelet** :- used for creation of pods or running app and if pod/app not running it informs one component in master (control plane) to take corrective actions
  - **Kubeproxy** :- networking like generating IP addresses and LB capabilities. To access the pods as outside user we can use kubeproxy.
  - **Container runtime** :- actually runs container

- **_Control Plane / Master Node_**
  - For any enterprise level components/tools there are specific standards like cluster creating pods. But to decide on which node pods to be created, is done by master.
  - Many components are there
  - **API Server** :- To take all incoming requests from users 9communication gateway0 or core component to do everything in k8s. Exposes our kubernetes to external world. It is heart of K8S. It basically keeps track of things happening on our K8S cluster. So if we'to talk to K8S cluster, we've to go through API server
  - **Scheduler** :- User tries to create pod, he tries to access API server which decides where to create component (on which node). To schcedule pods/resources on kubernetes worker node. Info decided by API server but acted by Scheduler. Used to schedule containers or pods and run the pod.
  - **etcd** :- We're deploying prod level app on K8s cluster, there has to be component inside kubernetes which acts as backup service or backing store of entire cluster information. It is key value store and entire K8S cluster info is stored as objects or key value pairs inside this etcd. We get the cluster related info from etcd. It is key value pair data store which stores all data of our K8S cluster.
  - **Controller Manager** :- As K8S supports autoscaling for which it has some controllers like replica sets which maintains state of our K8S pods. In K8S to make sure such components are always running, we have CMs
  - **Cloud Controller Manager** :- K8S can be run on cloud platforms like EKS, AKS, GKE. If we get request to create LB and if we send this request to K8S it has to understand underlying cloud provider. So K8S has to translate request from user to API request our cloud provider understands. This has to be implemented on CCM. CCM is open source. Any new cloud provider can take code from GitHub of CCM and write their logic so that in that cloud provider K8S is supported. If we're running K8s on premise, CCM is not at all required
 
- We can use 'kubectl' to give instructions to our K8S cluster to create resources and act upon them.
- Components in both nodes communicate with each other using Container Network Interface (CNI)

![image](https://github.com/user-attachments/assets/50e98585-dbd8-4f88-9aad-9b969ec603e5)


K8S Production Systems
- 
- This includes how devops engineers manage lifecycle like creation, upgrade, configuration and deletion of K8S clusters in production.
- We practice K8S on minikube or local cluster setups like K3S, kind, etc. But these are for development purposes, just local K8S clusters should not be used on prod.
- DevOps engineers should manage lifecycle of K8S clusters

- K8S is open source container orchestration solution platform having many distributions like EKS, Openshift, AKS, GKE, rancher,etc. Here we build user experience on top of K8S
  - If we have any issue with EKS, we can raise support ticket on amazon as we pay for it. If we install K8S on EC2, AWS asks to get support for it by EKS
  - If we've 100s of K8S clusters in organization, to run them there can be 100s of developers. If we ask each developer to create EKS cluster so all the revenue will go into EKS
  - So in staging or preprod we use K8S systems
  - So in production K8S is mostly used, then openshift, rancher, VM Ware and then EKS/AKS/GKE
 
- Difference between installing minikube and K8S directly on production
  - If we install K8S, we install it with all enterprise level capabilities. In minikube CPU and RAM itself are just 2 GB
 
- If we install EC2 instanced and install K8S on top if it to make it a cluster, that means we're managing the K8S cluster and amazon wont provide us any support for issues with K8S. Instead if we use KES, we get support from amazon as it is managed solution from AWS. EKS is basically distribution of amazon with additional wrappers, plugins and CLI option.

KOPS - Kubernetes Operations - To manage K8S Clusters
-
- Most widely used tool for installing K8S
- Create EC2 on personal laptop for which we require python3, AWS CLI and Kubectl
- Install KOPS then.
- Now we need to provide access to IAM user like AmazonEC2FullAccess, AmazonS3FullAccess, IAMFullAccess, AmazonVPCFullAccess (Required for IAM user only, not ADMIN user). For admin just run "aws Configure"

- For KOPS, we need S3 bucket as prerequisite as KOPS manages many K8S clusters and it can store all configs of them in S3 for storage.
  - Command :- **aws s3api create-bucket kops-shubham-storage --region us-east-1**
- Now to create K8S cluster
  - Command :- **kops create-cluster --name=demok8scluster.k8s.local --state=s3://kops-shubham-storage --zones=us-eat-1a --node-count=1 --nodesize=t2.micro --master-size=t2.micro --master-volume-size=8 --node-volume-size=8**
  - Here only cluster name is with domain name as k8s.local, in prod we can use amazon.com or anything like domain name and domain has to be configured in route53
 
  ![image](https://github.com/user-attachments/assets/b1e46a8f-08e4-485f-9e60-02f7e93bfbfc)

- To update the confurations of cluster created
  -  Command :- **kops update cluster demok8scluster.k8s.local --yes**       (Charges applicable)
 
- To configure domain in route53

![image](https://github.com/user-attachments/assets/727902fd-6aad-4d2a-9ad0-a81eb7c8c7c3)

