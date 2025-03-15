# Storage

- In cronjobs we defined volume and volume mounts. The pod running on server can get deleted anytime, get destroyed. So data inside it, that can also get deleted
- Suppose we create deployment which rolls our 2 pods. Even if we delete one of the pods, it will still maintain replica of pods (auto healing). But data inside the deleted container is gone
- Thus we need to persist data of our containers/pods for which we need to bind it with host. Container data to get bind with host so it doesnt get lost
- For it we need 2 things :-
  - Persistent volumes
  - Persistent Volume Claims

Persistent Volume
-
- On our host machine has some storage of 30 GB. We need to allocate 1GB so we create PV
  - In yml file, provide name, labels inside metadata. Inside spec provide capacity, access mode (read/write), to ratain persistent volume of host machine use reclaim policy, define storage class (of host machine). Thus we've to provide hostpath as well where we're allocating that 1GB.
  - Here in yml we havent defined namespace
 
![image](https://github.com/user-attachments/assets/1d0fa96c-7a4e-4b10-b901-28ba11ee813c)

- Here if we check PV is created or not :- **kubectl get pv**

![image](https://github.com/user-attachments/assets/75d2255f-ebeb-465f-8388-425d2dedd5fe)

- Now as we've created PV of 1 GB from our host machine, we've to claim it as well.

Persistent Volume Claim
-
- Claim is used to claim the PV data created.
- Here claim requests 1 GB from PV. As we've 1GB available in PV we've to claim it

- When we do kubectl get ns, we can see **local-path-storage** which gets bind with docker container and creates storage (defined in pv.yml and pvc.yml)

![image](https://github.com/user-attachments/assets/5d54c9e7-2261-4652-8b48-94d4693ca785)

- When we see pv list, we can see PV created is not available now, it gets bound as PVC has claimed it. local pvc is bound to local storage

![image](https://github.com/user-attachments/assets/595fcf7e-4c2b-45da-8b24-5c1c4f3f5975)

- Now we've to give this claim to the pods.
  - Here we've to create deployment whose data doesnt get deleted
  - Here inside deployment.yml, we can tell container to mount the volume
  - Inside it, the container html file is in mountPath which is /var/www/html which is to be mounted to local-pvc
  - Also volumes whose PVC to be local-pvc
  - This yml means nginnx container whose path /var/www/html will be mounted with volume. And create volume and give it name of claim
  - So the path /var/www/html will be with the volume and we'll bound that path with PVC so that the data gets stored at /mnt/data defined
 
  - Container path /var/www/html is bound with PVC 
 
![image](https://github.com/user-attachments/assets/81139c66-190d-40b9-94bd-9108b2e8778f)

- We need to define namepsace label in PV and PVC yml files above as well
