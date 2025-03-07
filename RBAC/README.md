# Kubernetes RBAC

- Role based access control
- It is very simple to understand but if not implemented properly, it becomes complicated to debug as it is directly related to security
- It is broadly divides into 2 parts :-  Users and Service

User Management
-
- If we're using k8s in minikube on which we've admin access as they are our local k8s clusters. But in organisation, as devops engineers we've our primary responsibility is to define access to k8s cluster (QA, DEv to have specific access acc to teams)
- Depending on role of person, we can define access using RBAC.

Service Account Management
-
- If we have pod what access should it have on cluster like configMaps, secrets, API servers
- If we've pod which is deleting content related to API server, to restrict it we can manage access to services on our K8S cluster using RBAC

In K8S to manage RBAC, we've 3 things
- User and Service Accounts
- K8S Cluster Roles
- Role Binding or Cluster Role Binding
  Similar to user management, we can also manage access to services using RBAC
