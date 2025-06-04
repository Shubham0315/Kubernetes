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
- Instead we can create ingress resource. If we've to manage all services using single IP address, we can use ingress. It also defines routing for our services and we can expose our cluster outside. We can do host based, path based routing.

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
- Create simple ingress resource, Define httpd service on port 80(defining traffic rules). It should only be accessed if user provides foo.bar.com

![image](https://github.com/user-attachments/assets/a2d7dd07-74bc-4ea5-a72d-fac5fb39d7e6)

- Create ingress using apply command
- We can have multiple types of ingress controllers in our cluster as well
- We can also curl the IP and pass host to tell ingress to only allow traffic if host is specific (-H for header). It becomes host based  :- **curl 192.168.59.100 -H 'host: foo.bar.com**

- If user tries to access foo.bar.com with context root(path) as /first then route traffic to http-svc and if he tries to access foo.bar.com with path as /second then route the traffic to meow-svc
  - Apply ingress and create
  
![image](https://github.com/user-attachments/assets/7db2ffb4-d363-4430-9bce-66a10f1129ec)

  - Now if we ry to access same URL, we'll receive 404 as we've not defined any path. As we've defined to only access when user tries to access one of the paths mentioned
  - Command :- **curl 192.168.59.100/first -H 'host: foo.bar.com'**

![image](https://github.com/user-attachments/assets/2ec2755a-c706-4f87-b456-26f09e6a747f)
![image](https://github.com/user-attachments/assets/ae60825a-dd21-48b6-8375-8f9fef19e4ad)

- To check logs of ingress :- **kubectl exec -it -n ingress-nginx deploy/ingress-nginx-controller cat /etc/nginx/nginx.conf**
  - ingress-nginx is namespace and ingress-nginx-controller is controller which updates configs in /etc/nginx/nginx.conf
  - When we check it we get below

![image](https://github.com/user-attachments/assets/8579f7a2-9de6-4f86-b780-9f3752bc9d26)

  - When someone tries to access location on /second then forward the request to specific service. SS above
  
Wildcard Ingress
-
- If we're on mail.google.com or drive.google.com, google.com is same. So for multiple services instead of creating multiple resources, create only one with wildcard (*.google.com) inside double quotes

![image](https://github.com/user-attachments/assets/7f105d59-262a-49f2-8caa-b301ec9ca6af)

- Apply the wildcard ingress (Here above ingress name is ingress-with-auth)
- To check created ingress :- **kubectl get ing**
- Ingress controller identifies ingress resource in yml and is updated IP address
- To access the same :- **curl 192.168.59.100/second -H 'host: $Name.bar.com**


- We can also set legitimate users to access domain. Here we can just annotate ingress to basic auth to achieve basic authentication

TLS
-
**1. SSL Passthrough**
- If we want to enable security, we can use SSL passthrough. It is a way by which client to server request is completely encrypted and our LB which is medium in between both just acts as passthrough. Request passes through LB without encryption or decryption.
- SSL passthrough passes encrypted HTTPS traffic directly to backend servers without decrypting traffics at LB

![image](https://github.com/user-attachments/assets/d45b4527-c275-484f-a516-c112af7198df)

- Our LB can be sometimes powerful or not which can act as firewalls or can do path based routing. But if we're using it as passthrough, we've to compromise on these features as LB act as just proxy nothing more.
- Also hacker can send request to server, LB doesnt even look at it and LB can miss the request and hacker can access the server directly as no encryption/decryption happen at LB level

- Suppose during vacation our service is receiving very high requests, service has to do decryption but here LB doesnt do anything. Every request coming is encrypted and service has top decrypt each request so we can see the latency. (Service has to do lot of work).

**2. SSL Offloading**
- This above problem can be handled by SSL offloading
  - It is type of TLS we can do with ingress. If we've SSL offloading enabled at ingress, decryption happens at LB level and it sends plain HTTP traffic to our service. So our service doesnt receive lot of load

![image](https://github.com/user-attachments/assets/8dd559ff-4374-493f-a350-d7674ee8b96f)

  - SSL offloading is not recommened in case of security as our LB sends a plain HTTP traffic for our application. So there is chance of vulnerability. If our LB is at edge of our network and it is passing plain HTTP traffic to our application, there can be a data theft possibility.
  - It is faster than passthrough so that application latency is reduced

**3. SSL Bridging**
- Also called as re-encrypt. Same as passthrough but here it doesnt send packets directly to the server as LB does some things for us. LB decrypts packet coming from the client and then it would re-encrypt.
- Every request coming from client, LB reads it and decrypts, looks into the information of the request, then before sending to server it re encrypts the packet. So both Client to LB and LB to app, both communications become secure

![image](https://github.com/user-attachments/assets/c912fc5e-d180-4b23-895b-2d0684bb847f)

- If hacker sends malware request, here due to decryption and LB reading the packet, it aborts the request. Validates malware attacks before sending to server
- Only here everytime request coming to server, server has to decrypt it

![image](https://github.com/user-attachments/assets/8c8d516e-f077-4297-9367-b1365bea69f1)

