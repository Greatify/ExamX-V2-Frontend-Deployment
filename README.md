# ExamX-V2 Frontend Deployment

This repository contains the deployment configurations and CI/CD workflows for the ExamX-V2 frontend application.

## Repository Structure

```
ExamX-V2-Frontend-Deployment/
├── .github/
│   └── workflows/
│       ├── ci-cd-dev.yaml    # Development environment CI/CD
│       ├── ci-cd-stg.yaml    # Staging environment CI/CD
│       └── ci-cd-prod.yaml   # Production environment CI/CD
└── k8s/
    ├── base/                 # Base Kubernetes configurations
    │   ├── kustomization.yaml
    │   ├── deployment/       # Deployment configurations
    │   ├── service/          # Service configurations
    │   ├── ingress/          # Ingress configurations
    │   ├── configmap/        # ConfigMap configurations
    │   └── autoscaling/      # Auto-scaling configurations
    └── overlays/             # Environment-specific overlays
        ├── dev/              # Development environment
        ├── stg/              # Staging environment
        └── prod/             # Production environment
```

## CI/CD Pipeline

The deployment process is automated using GitHub Actions with separate workflows for different environments:

### Deployment Trigger Methods
Workflows are triggered manually via GitHub workflow_dispatch with password protection for staging and production:
- Manual triggering for staging and production requires entering a deployment password
- Failed authentication attempts are logged and reported via Slack
- This adds an additional layer of security for deployments

### Development Environment
- Triggered manually with Docker image and SHA inputs
- Deploys the specified Docker image to the development Kubernetes cluster
- Updates the kustomization.yaml file with the new image tag
- Success/failure notifications via Slack

### Staging Environment
- Triggered manually with password protection
- Builds and pushes Docker images to Amazon ECR with tagged images
- Updates the kustomization.yaml file with the new image tag
- Deploys to the staging Kubernetes cluster
- Comprehensive security measures and notifications

### Production Environment
- Triggered manually with password protection
- Similar to staging process with production-specific configurations
- Additional security measures for production deployments

## Kubernetes Deployment

The application is deployed using Kubernetes with Kustomize for environment-specific configurations:

- **Deployment**: Manages the frontend application pods
- **Service**: Exposes the application within the cluster
- **Ingress**: Handles external access to the application
- **ConfigMap**: Manages environment-specific configurations including Nginx settings
- **Auto-scaling**: Ensures application scalability based on demand

## Prerequisites

- AWS account with ECR access
- Kubernetes cluster (EKS)
- GitHub repository access
- Required secrets configured in GitHub:
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - AWS_REGION
  - REPO_DISPATCH_TOKEN
  - SLACK_WEBHOOK_URL
  - DEPLOY_PASSWORD

## Deployment Process

1. For development deployments, specify Docker image and SHA in workflow inputs
2. For staging/production deployments, authenticate with deployment password
3. GitHub Actions workflow is triggered
4. For staging/production, Docker image is built and pushed to ECR
5. Kubernetes configurations are updated with new image tags
6. Application is deployed to the target environment
7. Deployment status is notified via Slack

## Monitoring and Alerts

- Deployment status notifications are sent to Slack
- Failed deployments trigger alerts with detailed information
- Success notifications include deployment details and tag the responsible user
- Security alerts for unauthorized deployment attempts
- Docker build failures are reported separately

## Security Considerations

- Manual deployments for staging/production are password-protected
- Secrets are managed through AWS Secrets Manager
- Secure token management for repository access
- Environment-specific configurations are isolated
- Unauthorized deployment attempts are logged and reported
- Environment files (.env) are cleaned up after builds

## Contributing

1. Create a feature branch
2. Make your changes
3. Submit a pull request

## Support

For any issues or questions regarding the deployment process, please contact the devops team.
