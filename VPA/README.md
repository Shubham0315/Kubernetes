# Vertical Pod Autoscalar

- VPA is a K8S feature that automatically adjusts CPU and memory resources requests and limits of the pod.
- Unlike HPA which adjusts replicas/pods based on metrics, VPA adjusts amount of resources allocated to pod to ensure that pod is adequately resourced without over-provisioning

- VPA is useful when workload requires dynamic resource adjustments for individual pods when:
  - We've to fine tune resource requests and limits of our apps
  - Workload cannot be scaled horizontally but may need more or less CPU/memory based on demand
 
- VPA adjusts CPU and memory resources of pod by continuously monitoring pod's resource usage. When it detects that pod is under or over provisioned, VPA will update pod's resources requests and limits. Pod is then restarted to apply new resource settings

Practical Demo
-
- Create deployment.yml and apply it

![image](https://github.com/user-attachments/assets/7b543e78-9754-4b18-86f9-1192e1d9bd03)

- Create VPA.yml like below

![image](https://github.com/user-attachments/assets/07ed8166-0624-4e6f-8b98-31e3e8255415)
