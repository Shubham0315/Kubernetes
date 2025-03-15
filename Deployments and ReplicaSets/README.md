Kubernetes Deployment
-
- If we can deploy our applications on K8S as pod, why do we need deployments?
- We make the pods, but real thing is to make those pods scalable for which we need deployment

Containers vs Pods vs Deployments
-
- For increased traffic we need group of pods which we can create defining replica sets, stateful sets or deployments
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

- Deployments also provide us with rolling updates. If weve 4 replicas/pods and we changed image of pod, RS gets changed. So in the meantime we might get downtime issue.
  - Deployment here provides rolling updates using which it says 1st pod is running, till then other pod is gettng updated so application doesnt face any downtime
- 
Practical Demo
-
- For one pod we cannot assign all traffic, so we create traffic. So to manage replicas there is 'Replication Controller'
- To list everything running (all resources) on our K8S cluster :- **kubectl get all**
- To list for all the namespaces :- **kubectl get all -A**
- If we create pod and it gets deleted or we delete it, user trying to access our application will face issues as they're outside our K8S cluster. After deleting if we try "minikube ssh" we cannot access the application and curl into it.

- In docker also same thing was there

- For this we can create deployments.
- For deployments, the apiVersion is **apps/v1**
- Here labels are used to give to pods so that deployment identifies the pods are of particular app.
- To select those pod, we also need selector. So we can tell selector to select whose app name is nginx. If app name and selector's app name gets matched, create our desired replicas
  - In template define pod template. Under it we can define pod spec like container name, image and port
 
Basically define pod with labels inside template. Define selector to fetch that pod label and if matching, create replicas mentioned. And in end define container spec for pod.
-

![image](https://github.com/user-attachments/assets/397e74c2-3e3b-4596-a6bc-1cb445198132)

- To create deployments :- **kubectl apply -f deployments.yml**
- As soon as we create deployment, pod also gets created along with RS

![image](https://github.com/user-attachments/assets/595b29d4-735e-4b04-a53d-bf0bc814691d)

- Deployment is an abstraction for us to implement auto healing and zero downtime in K8S
- Even if we delete pod and check if there is any pod is there, we see one pod getting initiated again as RS is 1 in deployment.yml for desired state

![image](https://github.com/user-attachments/assets/0c6972b4-bb57-44ff-ae3a-451f9c1e9ec7)

Thus auto healing is achieved here
-

- Namespace specific deployment and pod listing

![image](https://github.com/user-attachments/assets/2c8b4f78-5665-4fb2-963e-938873b1b2bb)
![image](https://github.com/user-attachments/assets/3f0413f4-5846-4d13-894c-f6b2f742f895)

- To scale the replicas usig CLI l- kubectl scale deployment/nginx-deployment -n nginx replicas=5

![image](https://github.com/user-attachments/assets/da9d857e-c99d-4ed6-bde3-4eb5a1b5a83f)

Stateful Sets
-
- Pod can get created, deleted, modified. Using SS, we can maintain state of each pod created using RS in sequence
- Replica sets and stateful sets maintain replicas. Only deployment is used to maintain rolling updated

Rolling Update
-
- If we've scaled our deployment and have created 5 replicas. If we check -o wide we get info like which pod is scheduled on which node
- These pods are using latest version of nginx. But we've to update to older version
- So to update :- **kubectl set image deployment/nginx-deployment -n nginx=nginx:1.27.3**      (nginx container name: image=image:tag)

![image](https://github.com/user-attachments/assets/fe1d8e65-7a63-42ba-8a5a-4522dec85e62)
![image](https://github.com/user-attachments/assets/4d68b540-a5e6-4ea4-9646-992c5e6f47c0)

- Here what happens is while updating pods, it doesnt stop all pod. It updates one by one stopping pods and perform rolling updates.
