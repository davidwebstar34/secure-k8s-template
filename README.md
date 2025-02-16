# secure-k8s-template

A secure, production-ready Helm chart template for Kubernetes deployments. This template is designed to help you quickly deploy applications with industry-standard security best practices. It includes pre-configured settings for secure container execution, resource management, health checks, and a sample CI/CD workflow for continuous security validation.

## Features

- **Namespace Management**: Optionally create a dedicated namespace for your deployments.
- **Service Account Setup**: Pre-configured ServiceAccount for streamlined RBAC.
- **Secure Deployment**: Includes best practices such as:
  - Running containers as non-root.
  - Read-only root filesystem.
  - Dropped Linux capabilities.
  - Pre-defined resource requests and limits.
- **Health Probes**: Configurable liveness and readiness probes for robust application monitoring.
- **Helm Templating**: Parameterize key values (namespace, image, replicas, etc.) via `values.yaml` for flexibility.
- **Rolling Updates**: Built-in rolling update strategy to minimize downtime during deployments.
- **CI/CD Integration**: Sample GitHub Actions workflow for security scanning using tools like kube-linter.

## Prerequisites

- **Kubernetes**: 1.16+
- **Helm**: 3.x
- **Git**

## Installation

1. **Clone the repository:**

   ```sh
   git clone https://github.com/yourusername/secure-k8s-template.git
   cd secure-k8s-template
   helm lint .
   ```

2. **Deploy the chart using Helm:**
   ```sh
   helm install my-secure-app . --namespace my-namespace --create-namespace
   DEPLOYMENT_NAME=$(kubectl get deployment -n secure-k8s-tenant -o jsonpath='{.items[0].metadata.name}')
    kubectl port-forward deployment/$DEPLOYMENT_NAME 8080:8080 -n secure-k8s-tenant
   ```
3. **Customize values:**

Adjust the settings in values.yaml to meet your application's requirements. You can also override these values during installation:

    ```sh
    helm install my-secure-app . --set deployment.replicas=3
    ```

## Chart Structure

    ```bash
    secure-k8s-template/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
        ├── namespace.yaml
        ├── serviceaccount.yaml
        └── deployment.yaml
    ```

## CI/CD Integration

This repository includes a sample GitHub Actions workflow located at .github/workflows/security-scan.yaml which leverages security scanning tools (e.g., kube-linter) to validate your Helm templates against security benchmarks.

Example snippet from the workflow:

```yaml
name: Security Scan

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  kube-linter:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Run kube-linter
        uses: stackrox/kube-linter-action@v0.2.0
        with:
          paths: "./" # Path to your Helm chart root
```

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests with improvements or bug fixes.

## License

This project is licensed under the MIT License.

## Acknowledgements

- Built with best practices inspired by the Kubernetes community.
- Security scanning powered by tools like kube-linter.

This README provides an overview of your project, clear instructions on how to get started, and details on the security features integrated into the template. Feel free to modify it further to match your specific needs or to include additional documentation as your project evolves. Happy charting!
