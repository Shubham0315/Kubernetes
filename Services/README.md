Kubernetes Services
-
- In K8s we usually deploy a deployment not a pod. For each deployment, we will create service in kubernetes

NO SERVICE Scenario
-
- As devops engineer we deploy pod as deployment in k8s. Deployment will create the replica set which will create pod (one or many depending on RS)
- Pods will be multiple to avoid all requests go to single pod while accessing application from users. This is not because of connections pod can take
- Lets say one replica of app handles 10 requests and we have 100 requests, we have to create 10 pods (define in yaml as replica sets no). If pod goes down, replica set will create new pod even before actual one is deleted or go down. Here pod comes up but IP address will be changed for app/pod
- We have to share these IP addresses of pods to application teams /test teams and they will try to access the application. If IP address is changes, user will say app is not working as he will be sending request to older IP address app. Although we used auto healing process here, problem occur and users will say application is not reachable
 
- Here we gave users the IP address of pod before getting killed and after resumption pod IP is changed so user faced the issue.

SERVICE Scenario
-
- Here on top of deployment we can create service. So instead of accessing pods, users can directly access service. So there wornt be any issue even if IP address gets changed (amazon.com)
- Instead of giving IP addresses to pods, we can create service on top of deployments. This service acts as Load Balancer which uses component in K8s which is "kubeproxy". So instead of using IP address, access pod/app using service name
- If user is not able to reach IP address of pod due to change in IP address, service will take request from user and forward it to the correct IP

- If we keep track of deployment which is creating 3 pods and one of the pod's IP address is changed. Here service will not bother about IP address it will come up with new process known as "LABLES and SELECTORS"
- As service cannot keep track of multiple IP addresses of pods, it introduces Lables and Selectors. For every pod getting created, devops engineers will apply label to it which will be common for all pods (not worry about IPs)
- So even if IP address changes, label will be same for pods. Replica set controller will deploy new pod with same yaml file which has auto healing. Service instead of keeping track of IP addresses, keeps track of labels called as SERVICE DESCOVERY PROCESS

- We create deployment using yml manifest, inside our metadata we create labels (app: payment). Deployment creates RS (lets say 2) both having same labels. If service goes down, IP will change but in yml has labels specified so pods will everytime come up with same labels.
- Even if new pod is created, svc will maintain service discovery for the same.

- When we create deployment, pod get created with IP address. Who has access to K8S cluster can hit our application. Service can expose app, it allows app to access outside k8s cluster for users.
- When we create service we can create svc of 3 types :- clusterIP, Node-port mode, LB using which we can expose 


- So basically there're 2 advantages of service
  - Load Balancing
  - Service Discovery
  - Expose apps to external world
 
1. ClusterIP Mode
- Using cluser IP, is by default behaviour of service, app is accessed inside k8s cluster. If service is created by this, only benefits are discovery and LB are achieved
- Cluster IP mode service is created in case of cluster and network

2. Node Port Mode
- Using nodeport , service will allow app to be accessible inside our organisation/network.
- So whoever has acces to your worker nodes of app, only they can access app if you create service as nodeport mode.
- Nodeport mode service is created in case of Nodes and VPC

3. Load Balancer
- service exposes app to external world
- If we create SVC using EKS we get ELB IP address. So anyone from the world can access this service as it is public IP
- LB only work on cloud providers.


Practical Demo
-
- To check if cluster is up and running :- **minikube status**

![image](https://github.com/user-attachments/assets/afa0d4bd-b1df-4970-9643-ab8b010c6b53)

- To create new resources on K8S, delete the existing ones except K8S default service

- Create dockerfile and build image using it. Include image name and tag while passing in CLI

![image](https://github.com/user-attachments/assets/3bc68239-4b7f-439e-91d0-7b575c4f8977)
![image](https://github.com/user-attachments/assets/40f0c840-13d7-48a3-8ea4-71d2b191ea17)

- Now we've to deploy this on K8S cluster. Create deployment.yml taking from google
- Here make sure labels are correct for each entiry as service will be looking for labels and selectors. When we create service we;ve to copy the label as is and use it is selector field of the service. Only then our service will be able to find out the pod. If we remove the label and its common for service and pod, then service will not be able to find the pod and we can see traffic loss
- Also the image we created using docker, use that name inside the image tag of container. Also make sure to add same port in yml as of dockerfile

![image](https://github.com/user-attachments/assets/87f16032-3ebe-45cd-9f8e-90d73556d57f)

- Now we can create deployment :- **kubectl apply -f deployment.yml**

![image](https://github.com/user-attachments/assets/a7e8e659-78a5-42ca-aff3-18f617e0f496)

- To understand what exectly happens when we run kubectl command, simply add verbose statement :- **kubectl get pods -v=7**   (V=9 to get info about API call)

- If we delete one pod, rs will create new pod with new IP address. The user who tries to access app on older IP, will get traffic loss. This is due to dynamic allocation of IP address, where IP address of newly created pod changes as per older
- Thats why we need service discovery mechanism. If k8s services identified pods using IP address it becomes wrong as it will face traffic loss as IP address are changed. K8s service identifies pods using labels and selectors so that when new pod comes up, its label will remain the same (although IP address might change). Label is like stamp

- For people within organization we can use service concept
- We can have application inside cluster and our organisation members trying to access app, we have to expose app on k8s worker node IP address. So that members in orgn can access app using worker node IP address  --> Nodeport mode can be used. For people outside our organisation, we need to create public IP address.
- Here we can use Node port and LB Mode

- There are 3 concepts for the service

**1. Expose Application to external world**
-
- Create service.yml. Keep labels/selectors same as pods created. Service will only look for pod having the label mentioned. Service will only bother about labels and selectors.
  - Copy the label name from "template" section of our deployment.yml
  - Change the target port in service.yml. (On target port our application runs)
 
 ![image](https://github.com/user-attachments/assets/7c9b2863-504f-453d-acb7-9d63f540d886)

  - Now apply the service :- **kubectl apply -f service.yml**

  - Check the service created :- kubectl get svc :- Checks application running, cluster IP.
  - As here we're using node port, we can see port mapping is done for the same
  - Clusetr IP : 80 is mapped with 30007.

![image](https://github.com/user-attachments/assets/a20edd99-7dad-4370-ba26-d6defac866fb)

  - Here if we dont want to use cluster IP and use node IP address (curl -L http://NodeIP ). On same computer, we can access it using browser using cluster IP as well. But for others laptop we cannot access as its external traffic needs LB. Minikube installs VM on laptop so its internal for us to access the aplication. We'll use port 30007 for service to route traffic to pods. We can also use this IP address to access the app using browser

![image](https://github.com/user-attachments/assets/ca0c24a3-7fcc-41dc-809f-0720e97cd3c5)

  -   But if we try to access this from outside, it wont work as we have not exposed app to outside world
  -   To make it accessible from outside edit the service :- **kubectl edit svc $name**
  -   Change the type to LoabBalancer.
  -   This wont work on minikube as LB mode is only supported on cloud

![image](https://github.com/user-attachments/assets/d84502f6-cba8-4aec-98f9-0f39b11440b8)

  - Now as we can see above, external IP will not be allocated as its minikube

**2. Service Discovery**
-
- Edit the service to change label name :- **kubectl edit svc $name**
- But better to vi service.yml and remove one character from previously configured label. So label of our pod and service is different
- Now check if service discovery able to detect pods. It wont as labels and slectors are different (even if its character mismatch)
- Now app wont be accessible even through browser nor curl as we couldnt connect to server
- So changing selector, service is not discoverable. After reverting back the changes, give it one minute as kubeproxy has to update rules and IPTable so as to be discoverable

**3. Load Balancing**
-
- Load balancing is required to distribute the loads on application/pods level.
- But pods dont have LB by default. If we create service we get LB

- We can install kubeshark for the same and open it
- Now we can run the curl command used earlier multiple times to check if the request of accessing application via curl is being segregated amongst the nodes/replicas of our pods (lest say our pods are 172.17.0.5 and 172.17.0.7)

![image](https://github.com/user-attachments/assets/8d023d1d-8afc-41ae-aa76-81f2bb245f4a)

- Thus here K8S service does the LB. From laptop we're executing 192.168 IP address from where request goes to minikube IP which is 172.17 from here it goes to service

- Packet flow :- **Application IP - Minikube IP - Service**
