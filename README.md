
# 3T WebApp - User Service (Helm Chart)

Welcome to the **3T WebApp User Service Helm Chart** repository! This project provides a Helm chart for deploying the `user-service` component of the 3T WebApp onto a Kubernetes cluster.

## 🚀 Project Overview

The `user-service` is a microservice responsible for handling user-related operations. This Helm chart enables easy, consistent, and scalable deployment of the service.

## 📦 Repository Structure

```
├── user-service-helm/
│   ├── charts/                 # Subcharts or dependencies
│   ├── templates/              # Kubernetes manifests templates
│   ├── Chart.yaml              # Helm chart metadata
│   ├── values.yaml             # Default configuration values
│   └── ...                     # Other Helm-related files
├── user-service/               # (Optional) Source code or Docker context
├── Jenkinsfile                 # CI/CD Pipeline definition
└── .gitignore                  # Ignored files
```

## ⚙️ Prerequisites

- Kubernetes Cluster (v1.21+ recommended)
- Helm v3.x
- Docker (for building images if modifying service code)
- kubectl configured with your cluster context

## 🛠️ Installation

To install the `user-service` using this Helm chart:

```bash
helm repo add 3t-webapp-user-service https://your-repo-url.com/
helm install user-service 3t-webapp-user-service/
```

Or, if deploying locally from this repo:

```bash
cd user-service-helm
helm install user-service .
```

## ⚙️ Configuration

Modify `values.yaml` to configure:

- Replica count
- Image repository and tag
- Resource requests and limits
- Service type and port
- Environment variables
- Ingress settings

Example override:

```yaml
image:
  repository: your-dockerhub-username/user-service
  tag: latest
  pullPolicy: Always

service:
  type: ClusterIP
  port: 8080
```

Apply with:

```bash
helm upgrade --install user-service ./user-service-helm -f custom-values.yaml
```

## 🧪 Testing the Deployment

Verify everything is running:

```bash
kubectl get pods
kubectl get svc
```

Test user APIs (replace `<NODE_PORT>`):

```bash
curl http://<CLUSTER_IP>:<NODE_PORT>/api/v1/users
```

## 🔁 CI/CD Integration

The included `Jenkinsfile` allows you to automate:

- Linting and testing
- Docker image builds
- Helm chart packaging and deployment

## 🙌 Contributing

Contributions are welcome! Please fork this repository, submit pull requests, and help improve the chart and documentation.

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

---

Happy Shipping! 🚢  
Maintained with ❤️ by [AnonyIIMessiah](https://github.com/AnonyIIMessiah)
