#  Kubernetes Config Maps and Secrets

Config Maps
-
- Suppose our backend application talks to DB and retrieves somes info and gives it back to user as per request. This info can be DB Port, username, DB pass, connection type, etc.
- This info can be retrieved using env variable as these values houldn't be hardcoded and can be changed over time. So user can get null or vague info. We can also save these entities inside file inside a path and retrieve from filesystem using OS modules.

- Suppose to retrieve DB port or connection type. As k8s deals with containers, user can get the info as part of container environment variable. K8S supports config maps
- So as devops or config management engineer we create config maps inside K8S cluster and put info like port in config maps, and mount this config map or use info of config map inside K8S pod
- So info can be stored inside pod as env variable or can be stored as file inside pod on container filesystem. And the info is retrieved using config maps.

- Creating env variables by creating pods, dockerfile is not recommended practice. So K8S go with config maps. So we collect info required by user from DB admin and create config maps where we can store the values. Then use data of config maps as env variables inside K8S pod.

- Config map solves problem of storing info which can be used by our application/pod/deployment later

Secrets
-
- Config maps solve the problem of strong data then why do we need secrets?
- Secrets deal with sensitive data like DB pass, DB user. If we put this in config maps with other things, this info gets stored inside etcd of K8S as objects. If any hacker gets access to etcd, they can retreive info. So our entore application is compromised in security aspects
- If we put data in secrets, K8S will encrypt the data at the rest before object gets stored in etcd. So hacker when trying to access etcd, if he doesnt know the decryption key, he can read info from etcd but apart from pass and user. He will get encrypted info which is of no use


- As user we create config maps using yml and inside yml we put details. Then apply yml and create config map. At the same time API server saves the info inside etcd as well.
  - For hacker if we're storing the sensitive info, he has 2 ways to retrieve info :- Accessing K8S cluster, access config maps etcd
  - He can do :- kubectl describe configmap :- so DB details get comporomised
  - He can go to etcd and as data is not encrypted he can get info from there
- These 2 problems are solved by secrets.
- While using secrets, K8S suggests to use strong RBAC. Give least privilege access. So suppose for developer, he can have access to pods, configMaps, deployments but doen't need to have access to secrets

- So using secrets. data is encrypted and we can use strong RBAC to make data more secure
