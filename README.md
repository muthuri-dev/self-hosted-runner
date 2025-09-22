# Self-Hosted GitHub Runner & Go API CI/CD

A complete demonstration of a CI/CD pipeline utilizing a self-hosted GitHub Actions runner. The project packages a Go API into a Docker image, pushes it to GitHub Container Registry (GHCR), and manages Kubernetes deployments via Helm.

## Project Structure

```
.
├── go-api/                     # Go API Application
│   ├── config/                 # Configuration management
│   │   └── config.go
│   ├── database/               # Database connection logic
│   │   └── connection.go
│   ├── Dockerfile              # Multi-stage container build
│   ├── docs/                   # API documentation (Swagger)
│   │   ├── docs.go
│   │   ├── swagger.json
│   │   └── swagger.yaml
│   ├── go.mod                  # Go module dependencies
│   ├── go.sum                  # Go module checksums
│   ├── handlers/               # HTTP request handlers
│   │   ├── health.go           # Health check endpoint
│   │   └── user.go             # User management endpoints
│   ├── main.go                 # Application entry point
│   ├── models/                 # Data structures
│   │   └── user.go
│   ├── repository/             # Data persistence layer
│   │   └── user_repository.go
│   └── services/               # Business logic
│       └── user_service.go
├── helm_charts/                # Kubernetes Deployment Configuration
│   ├── charts/                 # Chart dependencies (subcharts)
│   ├── Chart.yaml              # Helm chart metadata
│   ├── templates/              # Kubernetes manifest templates
│   │   ├── deployment.yaml     # Application deployment
│   │   ├── _helpers.tpl        # Template helpers
│   │   ├── hpa.yaml            # Horizontal Pod Autoscaler
│   │   ├── ingress.yaml        # Ingress configuration
│   │   ├── NOTES.txt           # Post-install notes
│   │   ├── serviceaccount.yaml # Service account
│   │   ├── service.yaml        # Network service
│   │   └── tests/              # Chart tests
│   │       └── test-connection.yaml
│   └── values.yaml             # Chart configuration values
├── playbook/                   # Ansible Automation
│   ├── inventory.ini           # Target hosts inventory
│   ├── playbook.yml            # Main playbook
│   ├── tasks/                  # Task definitions
│   │   └── main.yml
│   └── vars/                   # Variable definitions
│       └── main.yml
├── .github/workflows/          # GitHub Actions CI/CD
│   └── build-deploy.yaml       # Main workflow definition
├── LICENSE
└── README.md
```

## Prerequisites

- **Kubernetes Cluster** (Minikube, EKS, GKE, or AKS)
- **kubectl** configured for your cluster
- **Helm** (v3.x)
- **Self-Hosted GitHub Runner** registered with your repository

## Quick Start

### 1. Local Development

```bash
# Clone the repository
git clone <your-repo-url>
cd self-hosted-runner

# Run the Go application locally
cd go-api
go run main.go
```

The API will be available at `http://localhost:8080`.

### 2. Build and Test with Docker

```bash
# Build the container image
docker build -t go-api:local ./go-api

# Run the container
docker run -p 8080:8080 go-api:local

# Test the health endpoint
curl http://localhost:8080/health
```

### 3. Deploy with Helm

```bash
# Install the chart
helm install my-app ./helm_charts

# Upgrade deployment
helm upgrade my-app ./helm_charts

# View deployed resources
kubectl get pods,svc,ingress
```

## CI/CD Pipeline

The `.github/workflows/build-deploy.yaml` workflow automates:

**On push to main/develop or new tag:**
- Builds Docker image from `go-api/`
- Pushes image to GHCR with commit SHA tag
- Updates `helm_charts/values.yaml` with new image tag
- Commits updated Helm chart back to repository

**Manual deployment:**
- Use `helm upgrade` with the updated chart to deploy new versions

## Configuration

### Environment Variables

Configure database and application settings in `go-api/config/config.go`:

```go
// Database connection settings
DatabaseHost     = getEnv("DB_HOST", "localhost")
DatabasePort     = getEnv("DB_PORT", "5432")
DatabaseName     = getEnv("DB_NAME", "myapp")
DatabaseUser     = getEnv("DB_USER", "user")
DatabasePassword = getEnv("DB_PASSWORD", "password")
```

### Helm Values

Customize deployment in `helm_charts/values.yaml`:

```yaml
replicaCount: 2

image:
  repository: ghcr.io/your-username/go-api
  tag: "latest"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 8080

ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: api.example.com
      paths:
        - path: /
          pathType: Prefix
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Health check endpoint |
| `GET` | `/users` | Retrieve all users |
| `GET` | `/users/{id}` | Retrieve specific user |
| `POST` | `/users` | Create new user |
| `PUT` | `/users/{id}` | Update user |
| `DELETE` | `/users/{id}` | Delete user |

Swagger documentation available at `/swagger/index.html` when running locally.

## Architecture Overview

This project demonstrates a modern DevOps workflow with the following components:

### Application Layer
- **Go API**: RESTful service with clean architecture
- **PostgreSQL**: Database for persistent storage
- **Swagger**: API documentation and testing interface

### Infrastructure Layer
- **Docker**: Containerization with multi-stage builds
- **Kubernetes**: Container orchestration and deployment
- **Helm**: Package management for Kubernetes applications

### CI/CD Layer
- **GitHub Actions**: Automated build and deployment pipeline
- **Self-hosted Runner**: Custom execution environment
- **GHCR**: Container registry for image storage

### Monitoring and Operations
- **Health Checks**: Application and infrastructure monitoring
- **Horizontal Pod Autoscaler**: Automatic scaling based on metrics
- **Ingress Controller**: External traffic routing

## Security Considerations

- All container images are built using multi-stage builds to minimize attack surface
- Database credentials are managed through Kubernetes secrets
- Service accounts follow principle of least privilege
- Network policies can be implemented for additional security

## Troubleshooting

### Common Issues

**Build failures:**
- Ensure Go modules are properly configured
- Verify Docker daemon is running
- Check self-hosted runner has sufficient resources

**Deployment issues:**
- Validate Kubernetes cluster connectivity
- Verify Helm chart syntax with `helm lint`
- Check pod logs with `kubectl logs <pod-name>`

**Database connectivity:**
- Ensure database service is accessible
- Verify environment variables are correctly set
- Check network policies and firewall rules

## License

This project is licensed under the terms of the LICENSE file included in this repository.
