# Horizontal Pod Autoscalar

- HPA is a powerful feature in K8S that auto adjusts no of pods in deployment, stateful sets or replica sets based on observed CPU utilization or other custom metrics.
- Goal of HPA is to ensure that our application has right no of pods running to handle traffic while optimizing resource usage

- HPA works by monitoring a metric (CPU,memory) and adjusting no of replicas in deployment based on threshold we set. When we observe metric crosses defined threshold, HPA will increase or decrease number of pods to maintain desired performance

PRACTICAL DEMO
-
- The metrics-server is required for HPA to gather resource utilization data. To verify installation we can use below command
- Command :- **kubectl get pods -n kube-system**

- Create one deployment now with just 1 replica mentioned and apply it

![image](https://github.com/user-attachments/assets/d17557bb-15aa-4e41-82a9-3235c53e1e62)

- Now to create HPA, write yml file
- Here we can create HPA to scale no of pods based on CPU utilization
- In HPA.yml, inside spec mention from where we're taking target reference, set min and max replicas, provide based on which metrics we need to do HPA, its type and other parameters
- Here we've set conditions as below :-
  - It will maintain a minimum of 1 pod and can scale up to a maximum of 10 pods.
  - The HPA will adjust the number of pods to keep the average CPU utilization at 50%.

![image](https://github.com/user-attachments/assets/cd968451-a3ca-4951-ac62-5ec392e511ae)

- After applying this HPA.yml, we can check HPA is working or not :- **kubectl get hpa**

![image](https://github.com/user-attachments/assets/419519b5-e03a-45fd-8848-10b904b86b38)

- To see the HPA in action, you can simulate load by generating traffic to your nginx pods. One way to do this is by running a simple load generator like Apache Bench (ab) or using a stress tool to consume CPU.
- If After the CPU utilization increases and crosses the 50% threshold, Kubernetes will automatically scale the pods to meet the demand as our threshold is 50
- Then we can check no of replicas and hpa if it got autoscaled

-----------------------------------------------

- HPA ensures our application remains responsive under varying loads while optimizing resource consumption.
- By setting minimum and maximum no of replicas, we can ensure application is not over-provisioned thus improving cost efficiency

- Metrics can be :- CPU, memory, request count, response times.
- Here we can alsi integrate HPA for external use with prometheus

