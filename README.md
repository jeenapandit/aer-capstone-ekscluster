# This is an EKS project that uses eksctl, kubectl and Jenkins

Below docker images were used to the create the application and are in Docker Hub -
- jeenapandit/external-api:v2.0
- jeenapandit/internal-api:v2.0

## Jenkins Pipeline Stages -
1. Git Checkout to development branch
2. Creates EKS Cluster using cluster.yaml
3. Deploys resources using deployment and service yaml files in the **main** namespace
4. Verifies deployments are running
