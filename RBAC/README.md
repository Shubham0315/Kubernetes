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

- To create users, K8S doesnt deal with user management instead it offloads user management to identity providers
- We can simply login to K8S cluster and create service account, but same is not with user
- If organization is using AKS/EKS, to create users based on relevant access acc to team, for this K8S offolaods user management to identity providers

- Most of the apps have options like login with google. Here we dont have to create account for this application, user can access the app.
  - In K8S, we can pass certain flags to API server which act as OAuth server. If we're using EKS, we can use IAM users and login to K8S. So we need to create IAM OAuth provider.
  - So if we've users and group belonging to it, we can login to K8S using same username/group.

- Even if want to create cluster on EKS and integrate it with keyclock and integrate with GitHub where we can do user management.

- Creating service account is like creating pod. We can create serviceaccount.yml and provide attributes
  - By default there is a service account for our running pod using which apps talk to API server or any resources on K8S cluster. K8S attaches default svc-acc to our pod.
 
- To define access, K8S supports :- Role and Role binding
  - Once app is running, we've to grant access and create role. Lets say role should be like it to have access to pods, configMaps, secrets. Create role
  - To attach role to user, we will use role binding
  - So basically role is yml file where we write access which is required.
  - Now role is created and to attach role to service-account, we use role binding
 
We create svc-acc - create role - using role binding bind the svc-acc and role together
-

- Role takes care of permissions
- Service account takes care of users
- Role binding si for binding permissions to users

- If we create role for specific namespace, it is called as role and if we create role within cluster scope, it is cluster role

--------------------------------------------------------

# Core Concepts

- **Role** -	Defines a set of permissions (verbs on resources) within a namespace.
- **ClusterRole** -	Like a Role, but applies cluster-wide (across all namespaces or cluster-scoped resources).
- **RoleBinding**	- Binds a Role to a user/group/service account within a namespace.
- **ClusterRoleBinding** -	Binds a ClusterRole to a subject across the entire cluster.

- get, list, watch – Read operations
- create, update, patch, delete – Write operations
- exec, impersonate, bind, escalate – Special actions

- RBAC use cases
  - App reads Pods in one namespace	:- Role + RoleBinding
  - Admin monitors nodes :-	ClusterRole + ClusterRoleBinding
  - CI/CD job deploys to prod	:- Role/ClusterRole scoped to deployments, pods, etc.
