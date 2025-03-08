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


Practical Demo
-
- Create config map using yml

![image](https://github.com/user-attachments/assets/73d92299-efa2-4146-b908-f120c11b27d4)

- Now create cm :- **kubectl apply -f cm.yml**
- To check created config maps :- **kubectl get cm**
- To describe the cm :- kubectl decsribe cm test-cm

![image](https://github.com/user-attachments/assets/2e084a54-f2ba-445f-9a38-862007e3a676)

- We can see data entry in it. Now we've to take these fields from config maps and put them as env variable inside K8S pod

- Create K8S pod for this. Below is the deployment.yml. Apply that as well to create deployment which rolles out replica sets which will create pods for us

![image](https://github.com/user-attachments/assets/d2354601-6671-4755-9feb-d43d72a10f07)

- To check env variables of these pods :- **kubectl exec -it $NameOfPod -- /bin/bash**
  - Take name of pod from :- **kubectl get pods -o wide**

![image](https://github.com/user-attachments/assets/da3f3233-3022-4b9c-8edd-dac021c19732)

- Inside it if we search for env variable, we cannot search any

![image](https://github.com/user-attachments/assets/7ac1fb03-995d-4427-9876-54413cac28dd)

- This is due to just application is running and it doesnt have any info of the database port
- To get DB port info of mySQL, edit the deployment.yml. Add env: inside container and add DB port named env variable. Also define to get value of DB port from config map. Provide name and value in there

<img width="661" alt="image" src="https://github.com/user-attachments/assets/87004996-5506-4595-b5e4-0a493c261280" />

- When we deploy this K8S deployment, it should override existing pods and inside pods if we check env variables we should see new env variable named DB-PORT and value will be 3306. Below we can check for that
  - Apply the deployment. Check pods. Then exec into pod and check if env varibale is created
 
![image](https://github.com/user-attachments/assets/028fba65-a95a-43da-8d4f-cab62a2a4d9f)


How to use ConfigMpas inside your K8S pod?
-
- If for some reason we need to change our DB-PORT, we can do that change in cm.yml. But how our pod will know this as application inside pod will keep using previous port.
- For changing info, changing env variables inside K8S is not possible as container doesnt allow it. We've to recreate containers which we cannot do in prod as it might lead to traffic loss.

- Here we can use volume mounts.
  - We can using these entities as env variables we can use them as files.
  - Our config map info can be saved in file and dev can read info from file instead of env variables
 
- Create volume now.
  - At container level, create volume, give it a name. Allow it to read info from config maps named in cm.yml
  - Inside containers mount this created volume
  - Mounting volume means reading the volume inside K8S pod so provide name inside containers while mounting and provide folder mount path
 
  ![image](https://github.com/user-attachments/assets/ab77f57d-45cc-4f86-9477-ac51687e0353)

- Now apply the deployment and create
- Exec into pod and grep for env variable, we wont get results as we removed it from deployment.yml
- But we've mounted this in /opt. To check :- **ls /opt**
- We can check contents of that file mounted

![image](https://github.com/user-attachments/assets/ba701c57-b37f-4867-acb6-0fa53e237981)

- Now when we change port in cm.yml and apply the cm. K8S pod without getting restarted should know value of cm is changed
- We can check describe to see L1 verification

![image](https://github.com/user-attachments/assets/d9df331f-afd5-4e31-926f-e9e1ecf16c99)

- To check if pods got restarted :- **kubectl get pods**
  - It gives timestamp of start/restart
 
![image](https://github.com/user-attachments/assets/2ee6c8c6-3cec-4bf8-926c-8377d837d6b3)

- Now go inside pod and check if value of port is changed

![image](https://github.com/user-attachments/assets/971a6c06-e76a-468c-a97d-d96345abf627)

----------------------------------------------------------------------------------------

Secrets Demo
-
- Create secret :- **kubectl create secret generic test-secret --form-literal=db-port="3306"**
- This is to store DB pass/user

![image](https://github.com/user-attachments/assets/a00d3f29-3732-4258-bd47-2528fe6e4492)

- We can also create secret using secret.yml as well
- Check describe secret. We can see DB port as 4 bytes, encrypted format

![image](https://github.com/user-attachments/assets/7d880d3b-bc81-4fba-ac58-b7161a0e7c72)

- If we edit secret :- **kubectl edit secret test-secret**

![image](https://github.com/user-attachments/assets/8e8a87c8-cd3e-4272-bbd2-c4e68c43c6f6)

- To check if our Port value (secret value) is 3306 or not here :- **echo MxzMWnG== | base64 --decode**       (MxzMWnG== is value of secret in edit format, above command)

![image](https://github.com/user-attachments/assets/12ad1df8-648a-4935-82db-26c4394616c1)







