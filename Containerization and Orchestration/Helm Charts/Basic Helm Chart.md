## Basic Helm Chart Template

This Helm chart template offers a standardized and customizable method to deploy applications to Kubernetes, utilizing Helm, the package manager for Kubernetes. It consists of a `Chart.yaml` file for chart metadata, a `values.yaml` file for configuration values, and templated Kubernetes manifest files for a deployment and service.

### Chart.yaml

Create a `Chart.yaml` file with the following content to define the metadata for your Helm chart:

```yaml
apiVersion: v2
name: my-app
version: 0.1.0
```

### Values.yaml

The `values.yaml` file specifies configuration values that can be customized for deployment. Create a `values.yaml` with the following example content:

```yaml
replicaCount: 3
image:
  repository: my-app
  tag: "latest"
  pullPolicy: IfNotPresent
service:
  type: LoadBalancer
  port: 80
```

### templates/deployment.yaml

Under the `templates` directory, create a `deployment.yaml` file that uses templated values from `values.yaml`. Below is an example showing how to template the deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.nameOverride | default .Chart.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Values.nameOverride | default .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Values.nameOverride | default .Chart.Name }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: 80
```

### templates/service.yaml

Also, in the `templates` directory, create a `service.yaml` that dynamically sets values from `values.yaml`. Here is an example `service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.nameOverride | default .Chart.Name }}
spec:
  type: {{ .Values.service.type }}
  ports:
  - port: {{ .Values.service.port }}
    targetPort: 80
  selector:
    app: {{ .Values.nameOverride | default .Chart.Name }}
```

### How to Use

To deploy your application using this Helm chart, follow these steps:

1. **Prepare Your Helm Chart**: Ensure your `Chart.yaml`, `values.yaml`, and template files (`deployment.yaml` and `service.yaml`) are correctly set up in your Helm chart's directory structure.

2. **Customize Values**: Edit the `values.yaml` file to reflect your application's specific deployment and service configuration needs.

3. **Deploy with Helm**: Use the Helm CLI to deploy your application to a Kubernetes cluster with the following command:

    ```bash
    helm install my-app-release ./path/to/chart-directory
    ```

    Replace `my-app-release` with a name for your Helm release, and `./path/to/chart-directory` with the path to your chart directory.

4. **Verify Deployment**: After deployment, verify that your application is running as expected with Helm and Kubernetes CLI commands like `helm list` and `kubectl get all`.
