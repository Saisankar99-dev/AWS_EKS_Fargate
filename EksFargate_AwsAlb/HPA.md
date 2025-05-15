# Horizontal Pod Autoscaler (HPA) in EKS Fargate

## What is HPA?

Horizontal Pod Autoscaler (HPA) automatically scales the number of pods in a Kubernetes deployment or replica set based on observed CPU utilization (or other select metrics). It helps maintain application performance and cost efficiency by adapting to workload changes.

---

## How HPA Works in EKS Fargate

- EKS Fargate runs your pods serverless, without managing the underlying EC2 nodes.
- HPA monitors CPU or custom metrics and adjusts the pod count.
- Fargate then launches or terminates pods accordingly.
- Ensure your EKS cluster has the Metrics Server installed to enable CPU metrics collection.

---

## Prerequisites

- Kubernetes Metrics Server installed and running in your cluster.
- Proper RBAC permissions for HPA to access metrics.
- Your Deployment manifest to include resource requests and limits.

---

## Sample HPA Manifest

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: spring-boot-app-hpa
  namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: spring-boot-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
