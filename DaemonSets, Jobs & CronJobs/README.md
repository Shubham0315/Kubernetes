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

- Basically job is a K8S object that runs a pod to completion means
  - It runs task once or multiple times
  - Waits for task to succeed
  - Then end and doesnt restart

![image](https://github.com/user-attachments/assets/824c92f1-fa38-4cc5-9971-d822f9aaa34d)

- Here in yml we define that to run command and after executing wait for 10 seconds and task to get closed(sleep). Also attach restart policy as never so task doesnt run again

- First when we check job , it seems in running state and after 10 secs we can see status as completed

![image](https://github.com/user-attachments/assets/8283bc7f-46e3-4b2c-b64d-ab2e0b88e31e)

- So we can say pods also have lifecycle. It first goes to container creating state, running, terminating, completed. These are states of pod
- When we see the logs of job now, we see below output which is already defined in job.yml

![image](https://github.com/user-attachments/assets/2af1a99b-1e62-4e53-86cc-32630976cf5e)

- Once our job gets run, the pod goes from running state to completed

- If we delete job and then reapply it, we can see the job getting created again following the given lifecycle

![image](https://github.com/user-attachments/assets/df3cd6e9-60ba-420f-b6cb-2d05177d06e8)


Cron Jobs
- 
- A task that we can schedule.

- Lets write yml file for that where we need to take backup every minute
- * * * * * -> Min Hr Day DayOfWeek DayOfMonth, here * defines schedule
- */2 * * * *- every 2nd minute
- */2 */2  * * * - every 2nd minute of every 2nd hr
- In yml we define spec of job template which is template of single task which is our job. Basically its cronJob spec. In spec of cron job we schedule that job template and define metadata and labels in it

- Then we define pod specs. We can write multi line commands using >

- When we give data to container we need volume as well - volumeMounts using which we can store data by making folders
  - In mounts provide directory to mount so that data folder gets creaated
- Then write restart policy at container level

- Also provide volumes for volume mounts.
  - Suppose there is container inside our pod and onto our device we've to persist data from container so there is no data loss, thus volumes are created
  - So inside volumes, we define volume name and its type so that if directory not found it will create one

![image](https://github.com/user-attachments/assets/fde4d6b9-8a20-4902-914b-85afdf9b72cb)

- In above yml, 2 folders will get created backup and data, data folder content will get copied to backup folder. This runs every minute
- To check cronjobs getting scheduled or not :- **kubectl get cronjobs -n nginx**
- We can also check logs of backups

![image](https://github.com/user-attachments/assets/43d9db58-e39d-47d9-9943-e3495589360b)
![image](https://github.com/user-attachments/assets/78c7e65f-a8c5-4722-a0e8-213096f0cd70)

