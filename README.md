# Repository Description
This repository is a mono repo that contains Helm charts for the BA1 compute1 cluster core services.
The cluster is managed using the GitOps method and the ArgoCD tool.

## How it works?
bootstrapping the cluster is inspired by the [Cluster Bootstrapping](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) pattern.

there are three waves of deployment:
1. **Wave 1**: Deploying the argocd using `helm install` command when the `templates` directory is empty.
2. **Wave 2**: Creating the required secrets, in this project we need three secrets for bootstrpping:
    - `repo-creds`: secrets for accessing the git repository for argocd.
    - `kong-license`: secrets for kong enterprise license.
    - `kong-config`: secrets for kong configuration.
3. **Wave 3**: Creating the root Argocd app in the argocd chart `templates` that deploys every application present in the `apps` directory and call `helm upgrade`

after the bootstrapping process is done, the argocd will watch the `apps` directory and deploy the applications inside it.

**Note**: based on [argocd docs](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#manage-argo-cd-using-argo-cd) we place an app insides its chart's template to keep managing itself after the bootstrap.