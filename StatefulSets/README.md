# Stateful Sets

- When we work in production, for some apps even if our state doesnt get mainteained it will work as they're stateless(django/flask). But the DB of apps like mySQL, mongoDB are stateful apps.
- Thus data inside mySQL has to be persistent always, transactions to be timely. So we cannot manipulate with states of DB.
- So for stateless apps we use deployments, RS, daemon sets, etc. But for stateful apps we use "StatefulSets". Here also we use RS concept. So pods created will carry some state with it. So even when pod gets deleted and new gets up the state of deleted is transferred to new one. In SS the pods are numbered in sequence


- Firstly create one namespace and apply it

![image](https://github.com/user-attachments/assets/e13d03de-1dba-4b53-bdc6-6f00fa4ea6b6)

- Now write statefulset.yml like below. Add specs in which add replicas, selectors as matchLabels, templates in which provide labels of above, create container spec
- Here mysql container will ask for env-variables always. Provide mysql pass here as of now and provide value of it.
- But while creating SS of Mysql, storage required by pods has to be defined in SS yml
- Also we need volume claim templates. Provide name same as volume name here as well
- Write spec of volimeclaimTemplates
- So our volume claim name is mysql-data having access of readwrite who is requesting 1GB resource

![image](https://github.com/user-attachments/assets/c28aa19b-086d-44fc-87da-bbdf1368961e)

- Now just like stateful sets we also require service here
- This service will be headless for clusterIP will be none in spec as it will not be exposed its for internal use. This is as we dont want our mysql data to be accessible to outside world. Add selector as usual with port mapping
- This service is created as our mysql-service will e bound to one statefulset only

![image](https://github.com/user-attachments/assets/6957719b-741e-4219-a638-815957830fe0)

- Now go to ss.yml and add the service name in spec.


- Our every pod for our every replicas has bound our service, PVC template is also bound. So everything is defined in SS yml

![image](https://github.com/user-attachments/assets/e3a626ff-b9b8-4bf0-afaf-34987373f3b7)

- Now run service and then ss

![image](https://github.com/user-attachments/assets/2b9ca403-667a-40ed-bff4-c52cc8540ee7)

-------------------------------------------------------------

- Now exec into pod created
- Command ;- kubectl exec -it $nameofPod -n mysql -- bash
- This will ask for input : mysql -u root -p
- Using this we've created mysql deployment using SS which is scalable as well
- When we do "show databases", we can see the DB named "devops" is there already defined in ss.yml

![image](https://github.com/user-attachments/assets/ef5e1d5d-1cb6-4f38-9610-ca407dba2fee)

- Even if we delete the pod, we can see the pod of same name go created. Thats how K8S maintains state of resources with same name
- Here service, volume templates, env variables are tightly coupled to each other so whenever new pod gets created it carries state with it

- In deployment, replicas we can get diff names after recreation
