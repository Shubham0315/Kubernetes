<img width="1557" height="794" alt="image" src="https://github.com/user-attachments/assets/2bacfa93-e4eb-490a-beb4-d54ae28d822d" /># Probes

- Probes are used to monitor health of containers inside pod
- Probes are of 3 types :- Liveness, Readiness and Startup
- Probe is a kind of request that ensure our pod works fine
  - Suppose our pod is working on port 3000 but weve to make sure that its running on 3000.
  - So when probe gets created, it will request internally to port 3000
  - So if our pod is live or not is decided by Liveness Probes
  - If our pod is getting created and then will get ready, in that case Readiness probe is used
  - When our pod is getting started, so we can use Startup Probe
 
DEMO
-
- To check if our app is working on the mentioned port only, we can add liveness/readiness/startup probe below port config and wil ask httpGet to make request of on 80. If request comes there then liveness probe is working if not there we will get error. Done inside deployment.yml

<img width="1557" height="794" alt="image" src="https://github.com/user-attachments/assets/561528ce-903d-41d4-90ce-1ec4dba58fee" />

- If our liveness probe is created and our pod is ready, we can also create readiness probe like above

- Here httpGet is a probe to check HTTP endpoint
- **Initialdelayseconds** is a wait time to start container before probing to begin
- **periodSeconds** :- frequency of probing
- **failureThreshold** :- how many time containers must fail before considered as failed
- Now we can apply service and deployment. When we check for pods, we get pods list
- To checkif the liveness/readiness probe got triggered inside pod :- **kubectl describe pod $name**

- Probe check if our app works or not on given port and to check if our pods are successfully running or not
