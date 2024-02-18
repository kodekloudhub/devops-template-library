# Kubernetes Deployment and Service for High Availability

This configuration outlines the setup of a Kubernetes Deployment and Service for running high-availability applications. The deployment manages multiple replicas of your application, ensuring reliability and availability, while the service exposes these replicas externally on port 80 using a LoadBalancer, facilitating easy access.

## Deployment.yaml Content

Create a `Deployment.yaml` with the following content to define your Kubernetes Deployment. This setup ensures your application runs with three replicas, enhancing its availability.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 80
```

## Service.yaml Content

Create a `Service.yaml` with the following content to define the Kubernetes Service. This service exposes your deployment externally through a LoadBalancer, making your application accessible over the internet.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

## How to Use

To deploy your application and make it accessible using these Kubernetes configurations, follow the steps below:

### Prepare Your Deployment and Service Files

Make sure you have your `Deployment.yaml` and `Service.yaml` files ready. These files are crucial for defining the deployment and service configurations of your Kubernetes-managed application.

### Apply the Deployment

To create your application's deployment with the necessary replicas, execute the following command in your terminal:

```bash
kubectl apply -f Deployment.yaml
```

### Expose the Service

To make your application accessible externally, apply the `Service.yaml` file. This step will create a LoadBalancer service, enabling internet access to your application:

```bash
kubectl apply -f Service.yaml
```
