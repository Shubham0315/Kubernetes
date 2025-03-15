# Kubernetes Namespaces

- When we run docker container, we run it inside pod. There can be multiple containers inside one pod.
- K8S does auto scaling, healing for which we need deployments. To have access to external people, we will expose the pod using service
- For container, first we create pod from which we create deployment, then we create its service only then as a user we can access the application inside pod.

Container - Pod - Deployments - Services - User Access
-
- If there are multiple apps running on same cluster, to manage those things there can be a confusion.

- In K8S we've namespaces under which all the resources are isolated which wornt disturb resources in other namespaces

- To check active Namespace on our K8S cluster :- **kubectl get ns/namespaces**

![image](https://github.com/user-attachments/assets/81b0319d-ca9d-4d88-9614-48d6ee5429ec)

- If we dont create any NS and create resources on K8S cluster, they go to default NS
  - K8S cluster info go to '**kube-node-lease**'
  - Publically accessible services info goes to '**kube-public**'
  - System level services run in '**kube-system**'
 
- To check which pods are runnig in particular namespace :- **kubectl get pods -n $name**
  - K8S system components like master and data components are in '**kube-system**' namespace

![image](https://github.com/user-attachments/assets/9a9fdfce-73aa-49a6-920c-132ede285fc7)

- We can run different apps on different namespaces in K8S

Create New Namespace using CLI
-
- To create new NS :- **kubectl create ns $name**

![image](https://github.com/user-attachments/assets/9ebd785e-daf9-49a2-95d4-0a02106b8d43)

- Create container/pod now in new NS :- **kubectl create alpine --image=alpine -n nginx**

![image](https://github.com/user-attachments/assets/22950ab8-575b-4774-85de-a1bc5c4daee6)

Create New Namespace using YML
-
- To create nginx app, we need NS, deployment, pods, service. We can write all in manifest/yml file.

- Kind to be namespace, api version to be K8S version running now. Information to be given is in metadata section where we mention name of Kind9namespace0
- Then apply the yml file :- **kubectl apply -f namespace.yml**
- After applying our K8S object/resource gets created

![image](https://github.com/user-attachments/assets/ef828ba0-212f-4a15-bacd-85c3dadb2a15)
