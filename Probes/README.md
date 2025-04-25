# Probes

- Probes are of 3 types :- Liveness, Readiness and Startup
- Probe is a kind of request that ensure our pod works fine
  - Suppose our pod is working on port 3000 but weve to make sure that its running on 3000.
  - So when probe gets created, it will request internally to port 3000
  - So if our probe is live or not is decided by Liveness Probes
  - If our pod is getting created and then will get ready, in that case Readiness probe is used
  - When our pod is getting started, so we can use Startup Probe
 
DEMO
-
- To chec if our app is working on the mentioned port only, we can add liveness probe below port config and wil ask httpGet to make request of on 3000. If request comes there then liveness probe is working if not there we will get error. Done inside deployment.yml

![image](https://github.com/user-attachments/assets/ab60e941-3d0b-4545-a30c-82a0fa7093e8)

- If our liveness probe is created and our pod is ready, we can also create readiness probe

![image](https://github.com/user-attachments/assets/28746adf-cea2-4926-933f-cf73ece9247e)

- Now we can apply service and deployment. When we check for pods, we get pods list
- To checkif the liveness/readiness probe got triggered inside pod :- **kubectl describe pod $name**

- Probe check if our app works or not on given port and to check if our pods are successfully running or not
