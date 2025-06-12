# This is an EKS project that uses eksctl, kubectl and Jenkins

Below docker images were used to the create the application and are in Docker Hub -
- jeenapandit/external-api:v1.0
- jeenapandit/internal-api:v1.0

## Jenkins Pipeline Stages -
1. Git Checkout to development branch
2. Checks AWS credentials are set correctly
3. Deploys resources using deployment and service yaml files in the **development** namespace
4. Verifies deployments are running
