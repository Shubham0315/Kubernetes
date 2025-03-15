# Daemon Sets

- When we deploy pods and check status, we see pods getting assigned to differebt worker nodes
- Daemon sets ensure all our nodes must have at least 1 pod/replica running
- In our daemonset.yml file we dont need to mention replicas here

![image](https://github.com/user-attachments/assets/4a2d5fd7-125d-487b-a58a-a350f1ed378c)

- Now apply the daemon set. Here we dont mention replicas in yml, still daemon set creates pods on eaach worker node and ensure one pods is on each node

![image](https://github.com/user-attachments/assets/150281d8-bde9-45fe-b5c2-3490f964a639)


Jobs 
-
- Suppose we've to perform a single task from a container. That task to be a job which gets executed and gets finished. Tasks can be backup, update, patching and gets closed
- For this requirement, we create jobs

- Here while writing yml, kind will be job, vrsion will be batch/v1 as its batch job, then define metadata,etc
- In spec, to run job just once we can write completions, if we've to run job usig 2 parallel pods we can use parallelism, then write template
- Batch generally means one task will be performed or task will be performed in batches

![image](https://github.com/user-attachments/assets/824c92f1-fa38-4cc5-9971-d822f9aaa34d)

- Here in yml we define that to run command and after executing wait for 10 seconds and task to get closed(sleep). Also attach restart policy as never so task doesnt run again

- First when we check job , it seems in running state and after 10 secs we can see status as completed

![image](https://github.com/user-attachments/assets/8283bc7f-46e3-4b2c-b64d-ab2e0b88e31e)

- So we can say pods also have lifecycle. It first goes to container creating state, running, terminating, completed. These are states of pod
- When we see the logs of job now, we see below output which is already defined in job.yml

![image](https://github.com/user-attachments/assets/2af1a99b-1e62-4e53-86cc-32630976cf5e)

- If we delete job and then reapply it, we can see the job getting created again following the given lifecycle

![image](https://github.com/user-attachments/assets/df3cd6e9-60ba-420f-b6cb-2d05177d06e8)
