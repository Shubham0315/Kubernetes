Kubernetes Deployment
-
- If we can deploy our applications on K8S as pod, why do we need deployments?

Containers vs Pods vs Deployments
-
- We create containers using docker. To run the container, we provide specs using CLI.
- Kubernetes brings enterprise model to this. Instead writing this on CLI, we can write everything in yml manifest which is running specs of our docker container (parameters required to run pod). Pod can be single or multi-container for interdependency of applications. Multi containers promote shared networking between pods.

- K8S offers auto healing and auto scaling. Pod doesn't have capability to implement these things. They are offered by "DEPLOYMENTS"
  - If we've to do zero downtime deployment, bring in healing and scaling, then we should never deploy applications as pods instead deploy as deployment.
  - Deployment will deploy pods only but first it will create replica sets and then it will create pods
  - So don't create pod directly. Create deployment which will roll out replica sets which in turn will create number of pods which we defined in deployment.yaml
  - Replica set is a kubernetes **controller which ensures desired state and actual state of cluster are same**
  - Replica set is an intermediate resource needed between deployments and pods as inside deployment we can just define no of replicas of our pod as sometimes we need multiple replicas for our container due to traffic (LB or High availability concept)
  - Here inside deployment.yml we can set replicas as 2 and 2 replicas for pods will be created. Additionally as RS is a controller, it will ensure there will be 2 controllers always running means even if one of the pod (RS) gets deleted, K8S will implement auto healing using RS controller
  - We can update the yml if we need more RS
 
  - RS tracks controller behaviour in K8S (desired and actual state to be same)
 
End process is we create deployment which rolls out RS which creates no of pods defined in deployment.yml with auto healing capability on our K8S Cluster
-

Practical Demo
-
- To list everything running (all resources) on our K8S cluster :- **kubectl get all**
- To list for all the namespaces :- **kubectl get all -A**
- If we create pod and it gets deleted or we delete it, user trying to access our application will face issues as they're outside our K8S cluster. After deleting if we try "minikube ssh" we cannot access the application and curl into it.

- In docker also same thing was there

- For this we can create deployments.

![image](https://github.com/user-attachments/assets/397e74c2-3e3b-4596-a6bc-1cb445198132)

- To create deployments :- **kubectl apply -f deployments.yml**
- As soon as we create deployment, pod also gets created along with RS

![image](https://github.com/user-attachments/assets/595b29d4-735e-4b04-a53d-bf0bc814691d)

- Deployment is an abstraction for us to implement auto healing and zero downtime in K8S
- Even if we delete pod and check if there is any pod is there, we see one pod getting initiated again as RS is 1 in deployment.yml for desired state

![image](https://github.com/user-attachments/assets/0c6972b4-bb57-44ff-ae3a-451f9c1e9ec7)

Thus auto healing is achieved here
-
