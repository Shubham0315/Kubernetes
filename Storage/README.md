# Storage

- In cronjobs we defined volume and volume mounts. The pod running on server can get deleted anytime, get destroyed. So data inside it, that can also get deleted
- Suppose we create deployment which rolls our 2 pods. Even if we delete one of the pods, it will still maintain replica of pods (auto healing). But data inside the deleted container is gone
- Thus we need to persist data of our containers/pods for which we need to bind it with host. Container data to get bind with host so it doesnt get lost
- For it we need 2 things :-
  - Persistent volumes
  - Persistent Volume Claims
 
- On our host machine has some storage of 30 GB. We need to allocate 1GB so we create PV
  - In yml file, provide name, labels inside metadata. Inside spec provide capacity, access mode (read/write), to ratain persistent volume of host machine use reclaim policy, define storage class (of host machine). Thus we've to provide hostpath as well where we're allocating that 1GB.
