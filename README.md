# GitHub Actions Library

Reusable GitHub Actions workflows for CI/CD.

## Workflows

| Workflow | Description |
|----------|-------------|
| [terraform-quality](.github/workflows/terraform-quality.yml) | Terraform fmt, lint, and validate |
| [terraform-plan](.github/workflows/terraform-plan.yml) | Terraform init, validate, plan |
| [terraform-apply](.github/workflows/terraform-apply.yml) | Terraform apply with manual trigger |
| [docker-ecr](.github/workflows/docker-ecr.yml) | Build and push Docker image to ECR |
| [deploy-ec2](.github/workflows/deploy-ec2.yml) | Deploy to EC2 via SSM |

## Usage

### OIDC Authentication
All workflows now use **OpenID Connect (OIDC)** for AWS authentication. You must:
1. Configure an IAM OIDC Identity Provider for GitHub Actions in your AWS account.
2. Create an IAM Role with necessary permissions.
3. Add the IAM Role ARN as a secret named `ROLE_TO_ASSUME` in your repository.
4. Ensure your calling workflow has `permissions: id-token: write`.

```yaml
jobs:
  deploy:
    permissions:
      id-token: write # Required for OIDC
      contents: read
    uses: aakash/github-actions-library/.github/workflows/docker-ecr.yml@main
    with:
      ecr_repository: my-app
    secrets:
      ROLE_TO_ASSUME: ${{ secrets.ROLE_TO_ASSUME }} # IAM Role ARN
      AWS_REGION: ${{ secrets.AWS_REGION }} # Optional
```
