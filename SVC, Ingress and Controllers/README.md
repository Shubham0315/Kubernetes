# Kubernetes Service

- It is an abstract way to expose and application running on set of pods as network service

- Suppose in our K8S cluster we've deployed "checkout" application as a pod, we get IP for it. One more application named "payments" have another IP address
- Here checkout has to communicate with payments for any kind of transactions
  - It can connect to payments using cluster IP assocaited with pod(using pod endpoint). But IP address being dynamic and if pod goes down, next time while coming up new IP address will be allocated. So checkout while connecting with payments will throw an error
  - To avoid this we can use service. Its just a proxy and it identifies pods using labels and selectors

![image](https://github.com/user-attachments/assets/ce217790-960d-45e2-a3e0-a6b6a60c6bef)
 
- Ways to create services
  - Cluster IP
  - Node port mode
  - Load Balancer service
 
1. Service as Cluster IP
- IP range stays within cluster so if someone wants to access payments app here, cluster IP wouldnt be accessible

2. Node port
- If we want to expose the service outside, we can access our pod or service using node port.
- Kubeproxy has port range, it binds one of the ports to our service and it updates IP tables
- If someone has to access nodeName:port , forward the request to payments application. So we can access application from outside of K8S cluster
- Here drawback is our node might not access from outside. We cannot expose multiple ports on our firewall

3. Load Balancer
- Creates external IP address for us
- Suppose we've K8S cluster on AWS EKS and we expose service as LB. CCM generates external IP address (static) for our service and using which we can access our service

--------------------------------------------------------------------

Kubernetes Ingress
-
- Suppose we've an ecommerce site where 200-300 services to be exposed. For each service using LB type, we create 200-300 static IPs for which cloud provider will charge us
- Instead we can create ingress resource. If we've to manage all services using single IP address, we can use ingress. It also defines routing for our services and we can expose our cluster outside. We can do host bases, path based routing.

![image](https://github.com/user-attachments/assets/715ef068-1064-48ce-bcf9-da567b2badb0)

- In above SS, we've ingress resource which defines routing rules for services.
  - We can say specific service to be accessed on specific domain name
  - Each service can be accessed on different context paths
 
- So apart from creating service we need to create ingress.yml
- Ingress cannot work on its own, it needs ingress controller to be created

- Ingress controller will watch ingress resource and it will update/configures Load balancers

- Suppose we've nginx ingress controller. On our K8S cluster we install nginx ingress controller using helm. It will watch for ingress resources on cluster and it updates nginx.conf(file to update routing details of nginx).
  - Here along with ingress controller, nginx application LB is also deployed
  - Some LBs may be inside the cluster or outside
 
![image](https://github.com/user-attachments/assets/5d4b572c-6c0b-4ac6-a7d0-1ecf5408c2e5)

- With ingress we can do host based or path based routing

--------------------------------------------------------------------

Ingress Demo
-
- Create simple ingress resource
