# Kubernetes Ingress

- Service was used to discover, LB and exposing application to external world.
- Initially before 2015, people were creating deployment, pod where deployment will provide auto healing auto scaling, we can create service on top of pod so that we can expose app within or outside k8s cluster with load balancing. There wasn't a concept of Ingress initially.
- 
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
- 
