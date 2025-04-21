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
    │   ├── deployment/       # Deployment configurations
    │   ├── service/         # Service configurations
    │   ├── ingress/         # Ingress configurations
    │   ├── configmap/       # ConfigMap configurations
    │   └── autoscaling/     # Auto-scaling configurations
    └── overlays/            # Environment-specific overlays
```

## CI/CD Pipeline

The deployment process is automated using GitHub Actions with separate workflows for different environments:

### Deployment Trigger Methods
All workflows can be triggered manually via GitHub workflow_dispatch with password protection:
- Manual triggering requires entering a deployment password
- Failed authentication attempts are logged and reported via Slack
- This adds an additional layer of security for deployments

### Development Environment
- Triggered on pushes to the `dev` branch or manually with password
- Builds and pushes Docker images to Amazon ECR
- Deploys to the development Kubernetes cluster
- Includes language compilation and translation updates

### Staging Environment
- Triggered manually with password protection
- Similar process to development with staging-specific configurations

### Production Environment
- Triggered manually with password protection
- Production-grade deployment with additional security measures

## Kubernetes Deployment

The application is deployed using Kubernetes with the following components:

- **Deployment**: Manages the frontend application pods
- **Service**: Exposes the application within the cluster
- **Ingress**: Handles external access to the application
- **ConfigMap**: Manages environment-specific configurations
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

1. Code changes are pushed to the respective branch or deployment is manually triggered
2. For manual deployments, password authentication is verified
3. GitHub Actions workflow is triggered
4. Docker image is built and pushed to ECR
5. Kubernetes configurations are updated
6. Application is deployed to the target environment
7. Deployment status is notified via Slack

## Monitoring and Alerts

- Deployment status notifications are sent to Slack
- Failed deployments trigger alerts with detailed information
- Success notifications include deployment details
- Security alerts for unauthorized deployment attempts

## Security Considerations

- Manual deployments are password-protected
- Secrets are managed through AWS Secrets Manager
- Protected branches are used for critical environments
- Secure token management for repository access
- Environment-specific configurations are isolated
- Unauthorized deployment attempts are logged and reported

## Contributing

1. Create a feature branch
2. Make your changes
3. Submit a pull request

## Support

For any issues or questions regarding the deployment process, please contact the devops team.
