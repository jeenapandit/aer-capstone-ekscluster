# This is an EKS project that uses eksctl, kubectl and Jenkins

Below docker images were used to the create the application and are in Docker Hub -
- jeenapandit/external-api:v1.0
- jeenapandit/internal-api:v1.0

The Docker images were built from below repos-
- https://github.com/jeenapandit/aer-sample-external
- https://github.com/jeenapandit/aer-sample-internal

## Jenkins Pipeline Stages -
1. Git Checkout to development branch
2. Checks AWS credentials are set correctly
3. Deploys resources using deployment and service yaml files in the **development** namespace
4. Verifies deployments are running
