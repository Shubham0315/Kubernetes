# Daemon Sets

- When we deploy pods and check status, we see pods getting assigned to differebt worker nodes
- Daemon sets ensure all our nodes must have at least 1 pod/replica running
- In our daemonset.yml file we dont need to mention replicas here

![image](https://github.com/user-attachments/assets/4a2d5fd7-125d-487b-a58a-a350f1ed378c)

- Now apply the daemon set. Here we dont mention replicas in yml, still daemon set creates pods on eaach worker node and ensure one pods is on each node

![image](https://github.com/user-attachments/assets/150281d8-bde9-45fe-b5c2-3490f964a639)


Jobs 
-
- Suppose we've to perform a single task from a container

