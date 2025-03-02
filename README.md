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
  - If someone kills our container, application inside it wont be accessible unless someone start the container/act upon the container. This process is called "AUTO HEALING". This has to be done without user's manual intervention, container should start by itself
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
 
K8S Doesnt support advanvced Load Balancing capabilities
