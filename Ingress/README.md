# Kubernetes Ingress

- Service was used to discover, LB and exposing application to external world.
- Initially before 2015, people were creating deployment, pod where deployment will provide auto healing auto scaling, we can create service on top of pod so that we can expose app within or outside k8s cluster with load balancing. There wasn't a concept of Ingress initially.

- Still there were some practical problems with k8s use.
  - What people used to do is on their virtual machines or physical servers, they deploy the app where they use LB like nginx, FI which ere of enterprise level

- Enterprise LB offers very good LB capabilities like below:-
  - Ratio based LB :- send 3 requests to pod1 and 7 to pod2
  - Sticky sessions :- if one req to pod1 then all goes to pod1
  - Path based LB
  - Domain/host based LB
  - White listing :- only specific customers to access app
  - Blacklisting :- dont allow users from specific country, used to define traffic
 
- When people migrated from VM to K8S, they realised service was offering LB but it was simple round robin LB

- Lets say if we've to route user to nginx service when he requests for /nginx, route him to alpine service when he asks for /alpine, for this we can use ingress
- Using Ingress we can re-route the traffic on our K8S cluster

- Suppose we've pods inside a cluster and pods are being managed by a ddeployment. To access the deployment we need service so we we create the service for this deployment. Similarly we've other deployment having pods and we've service created
  - Here to manage these microservices we need ingress.

Problem-1 :- Enterprise and TLS LB were missing
-
- When people migrated from virtual machines to k8s, k8s was offering auto healing, auto scaling, expose app to real world . But LB mechanism it was providing was of type "ROUND ROBIN"
- Round Robin :- If you are sending 10 requests using kube proxy, 5 requests are sent to pod1 and 5 to pod2 , Very simple LB
- But commercial enterprise level LB provide 100's of features which K8s was not supporting Enterprise LB features. Services in K8S was not offering enterprise level capabilities

Problem-2 :- For each LB mode service, cloud provider charges them for each and every IP address 
-
- We expose our app to external world using LB. We create our service as LB mode. For hundreds of services, when we create them as LB Mode, cloud provider was charging them for each IP address as these were static and public. 
- On physical servers or VMs, there used to be one LB for no of apps. So people used to configure if request is coming to specific IP send it to specific app and expose these LB with static public IPS
- So here only one IP address was used to expose but in K8S we expose 1000s of IPs and get charged :- problem

So while using service, these 2 problems arise. These things are missing in service
- Enterprise and TLS LB (VMs have very good LB capabilities)
- For LB service, cloud provider will charge for every service

Customers complain was on VM there was good LB capabilities making applications very secure and less costly. But when moved to K8S, problem arose. Thus INGRESS was implemented.
-
- People on VM used LBs like Nginx, F5, HAProxy. K8S allows to create ingress resource for the LB. Not for each LB, K8S will tell LB providers to create ingress resource so all LB companies will create ingress controller
- If we create ingress resource on K8S cluster and we need path based routing. We can come to K8S cluster, create path based routing , create yaml and define path based.
- To implement this, to decide which LB to use, it asks LB provide to create ingress controller. And we as users will deploy ingress controller on K8S cluster.
- After deploying, devops engineer will create ingress yaml resource for K8S services.
- Ingress controller will watch for ingress resource and provide use with path based routing

- We're creating pod on K8S cluster using yaml. Kubelet here deploys our pod on one worker node. Similarly while creating service.yml, there is kubeproxy will update ip table rules.
  - So for every resource we create in K8S there has to be component watching that resource and performing required action
- Similarly for ingress, there has to be resource or controller.

- To implement the same, K8S comes up with an architecture like "Users will create ingress resource". LB providers will write their own ingress controllers and provide how to install. So as user we also have to deploy ingress controller now.

Ingress controller is just a LOAD BALANCER
-
- As user instead of just creating ingress resource, we also have to deploy ingress controller on K8S cluster. After that we create ingress resource depending on our need (path based routing, TLS based, host based)
- Here one time activity is to decide which controller/LB we want

Practical Demo
-
- Create ingress resource and setup host based LB with it
- Go to ingress documentation and copy ingress syntax
- If anybody wants to reach our application, if they hit on host defined in ingress.yml/bar, they should reach service

![image](https://github.com/user-attachments/assets/39679670-69ee-42eb-a6f7-55eebaae375c)

- Now to deploy ingress :- **kubectl apply -f ingress.yml**
- Ingress gets created but address field is empty. So if we try to hit URL, no output

![image](https://github.com/user-attachments/assets/548b0b29-1768-4744-805d-289870a778a6)
![image](https://github.com/user-attachments/assets/2912a9e9-ef04-4dd5-8fa7-1b450cc68533)

- This error is as we havent created ingress controller. We've only created ingress resource

- First intsall nginx ingress controller as its lightweight and simple for us.
- Search for nginx K8S ingress controller and run below command to create ingress controller :- **minikube addons enable ingress**

![image](https://github.com/user-attachments/assets/695a022e-aa3f-45f3-a39e-d3b64ad79d84)

- In the end ingress controller is also a pod. To check in which namepsace it is installed :- **kubectl get pods -A | grep nginx**
  - Here ingress-nginx is namespace and nginx is controller name

![image](https://github.com/user-attachments/assets/0dca691a-c04c-41eb-ba55-92e650f6e70d)

- To check logs of ingress controller :-** kubectl logs $controllerNameWithId -n $IngressName**
- This command displays ingress resource created
- We can get resource created in logs at end where we can see it is synced means it goes to nginx LB configuration in nginx.conf file and it updates ingress configurations
- Now when we check ingress, we can see address field is populated in the output

![image](https://github.com/user-attachments/assets/1fb26121-d96e-418c-8bff-f6fa322756ac)

- Now ingress resource we created using yml can be accessed on foo.bar.com as in yml as we've used ingress controller which updates configurations

- We also have to do one configuration on local K8S cluster. Edit /etc/hosts on minikube as we've not done domain mapping. So foo.bar.com has to be mapped wwith 192.168.64.11 which is ingress IP
  - When we try to ping foo.bar.com, IP address doesnt exist. Add it in /etc/hosts file

<img width="634" alt="image" src="https://github.com/user-attachments/assets/f43e064c-2db9-42a8-a142-a26fb6ea28b5" />

- So domain will be resolved on the IP
- This is done only on local not prod.
