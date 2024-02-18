## Horizontal Pod Autoscaler (HPA) Template

Horizontal Pod Autoscalers (HPA) automatically adjust the number of pods in a deployment, replicaset, or statefulset based on observed CPU utilization or other selected metrics. This template facilitates the quick setup of an HPA, promoting efficient resource use and enhanced response to load variations.

### HPA.yaml

To define your HPA, create an `HPA.yaml` file with the following content. Replace the placeholder values (e.g., `<hpa-name>`, `<namespace>`, `<deployment-name>`) with your specific application details.

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: <hpa-name>
  namespace: <namespace>
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: <deployment-name>
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

### Customization Instructions

- `<hpa-name>`: Name your Horizontal Pod Autoscaler.
- `<namespace>`: Specify the namespace where your target deployment resides.
- `<deployment-name>`: The name of the deployment you wish to autoscale.
- `minReplicas` and `maxReplicas`: Define the minimum and maximum number of pods that can be automatically scaled.
- `averageUtilization`: Set the target CPU utilization percentage that triggers the scaling action.

### How to Use

To implement your HPA in a Kubernetes environment, follow these steps:

1. **Prepare Your HPA Configuration**: Edit the `HPA.yaml` file with your specific details, ensuring that you replace all placeholder values with those relevant to your deployment.

2. **Apply the HPA**: Deploy your HPA to your Kubernetes cluster using the following command:

    ```bash
    kubectl apply -f HPA.yaml
    ```

3. **Verify the HPA**: After applying the HPA configuration, you can verify its status and functionality with:

    ```bash
    kubectl get hpa -n <namespace>
    ```

    Replace `<namespace>` with the namespace of your HPA. This command provides information about the HPA, including its target metrics and current status.
