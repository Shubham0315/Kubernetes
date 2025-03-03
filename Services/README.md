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
- Here on top of deployment we can create service. So instead of accessing pods, users can directly access service. So there wornt be any issue even if IP address gets changed
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
