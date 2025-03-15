# Kubernetes Namespaces

- When we run docker container, we run it inside pod. There can be multiple containers inside one pod.
- K8S does auto scaling, healing for which we need deployments. To have access to external people, we will expose the pod using service
- For container, first we create pod from which we create deployment, then we create its service only then as a user we can access the application inside pod.

- Container - Pod - Deployments - Services - User Access

- If there are multiple apps running on same cluster, to manage those things there can be a confusion.

- In K8S we've namespaces under which all the resources are isolated which wornt disturb resources in other namespaces

- To check active Namespace on our K8S cluster :- **kubectl get ns/namespaces**

![image](https://github.com/user-attachments/assets/81b0319d-ca9d-4d88-9614-48d6ee5429ec)

- If we dont create any NS and create resources on K8S cluster, they go to default NS
  - K8S cluster info go to 'kube-node-lease'
  - Publically accessible services info goes to 'kube-public'
  - System level services run in 'kube-system'
 
- To check which pods are runnig in particular namespace ;- kubectl get pods -n $name
  - K8S system components like master and data components are in 'kube-system' namespace

![image](https://github.com/user-attachments/assets/9a9fdfce-73aa-49a6-920c-132ede285fc7)

- We can run different apps on different namespaces in K8S

- To create new NS ;- kubectl create ns $name
