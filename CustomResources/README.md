# Kubernetes Custom Resources

- There are 3 things
  - Custom Resources
  - Custom resource definition known as CRD
  - Custom controller
 
- On our K8S cluster there are some resources like deployment, service, pod, config maps, secrets. But if we want to extend API of kubernetes or want to introduce new resource to kubernetes as we might feel we need some functionality enhancement for some resources like K8S Doesnt support advanced security aspects or we want to extend GITOPS in k8s. So here we use CR, CRD, CC
- Deploying CRD and CC is done by DevOps engineer while CR can be deployed by user or DevOps engineer

- Our organization has deployed application as deployment and created service, ingress for the same. Deployment uses service, config maps. User is also able to access the application through ingress. Traffic is flowing in and out of our K8S cluster
  - But we can see there are many things beyond these K8S services like Istio which adds service based capabilities to K8S cluster, ArGoCD to add GitOps, keyclock providing tight access management to K8S cluster
  - These are used to enhance behavior of K8S cluster.
  - For each of the capability K8S cannot add logic for these new applications into CCM. K8S already implemented logic for deployment, service, cfg maps. Practically impossible for K8S
  - So if we've to add new capabilities to K8S, we'll allow users to extend API of K8S. So we can add new API resource to K8S (using CRD, CR, CM)
 
Custom Resource Definition (CRD)
-
- Suppose a company says we've to extend capabilities of K8S, K8S will ask that company to create CRD means to create new API to K8S.
- So company will create CRD using yml where we define what user can create. If we define any new field in yml we might get error as "Field XYZ is not known". This is due to K8S knows definition of yml file of that particular resource
- For custom resource creation we need to submit API to K8S.
- So the company who want to extend capabilities will provide complete yml file having all possible options that they support. CRD is yml file used to introduce new type of API to K8S which have all fields user can submit in custom resource.

- In deployment.yml we define api version, kind, metadata, etc. This is K8S resource where K8S has resource definition in API server or CM. Resource definition will validate resource we've created is right or wrong.
- Similarly in CRD, user will create custom resource. Suppose istio has custon resource called "Virtual Service". We can define api version, kind, spec, etc in yml file. This virtual service is our custom resource
- This CR is validated against CRD which istio have created which we as devops engineer will deploy on K8S cluster so our cluster is extended

- CRD Is used to
  - Extend capabilities of K8S API
  - Validate

Custom Controller (CC)
-
- Now user has created CR inside K8S cluster and validated against CRD. So there has to be CM or Custom K8S Controller which is already deployed inside K8S cluster
- So once we deploy our CR, controller will watch CR and do action

- If organization wants to use Istio, devops engineer will deploy CRD on K8S cluster (Virtual Service is CRD for Istio).
- User will also go to Istio docs and he will create custom resource. Inside namespace he will create CR. While creation API server will intercept the request they'll validate it against CRD and deployed on K8S cluster
- If we deploy ingress resource without ingress controller, nothing will happen.
- Here CR has to be watched by someone. So devops engineer will deploy Custom Controller. CC can be created across the cluster or for specific namespace
- DevOps engineer will deploy CC inside namespace. CC is verified by controller and controller will perform required action
- Istio controller deployed will read CR and perform action



How to write a Custom Controller
-
- Popular medium to write is using GoLang. Initially client-go was used to interact with K8S API
- To write custom controller, we've to talk to K8S so client-go inside K8S API server will interact with API server
- We can also write in python, java which came later

- Using GoLang as our programming language in our K8S API, we'll interact with client-go and then setting watchers for it. When we do update, delete, create, K8S will come to know using the watchers.
- But for our custom K8S controller, we've to create own watchers.
- We can setup watchers using controller runtime as well. We can setup watchers for virtual service so any action is performed, watchers will notify client-go. 
