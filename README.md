# Helm chart template

Helm chart template project

## Bootstrap

### Prerequisites

set alias for kubectl

```bash
alias k=kubectl
```

download and install argocd-vault-plugin on your local machine

<https://github.com/argoproj-labs/argocd-vault-plugin>

Adapt the secrets below and export the variables for the vault plugin.

```bash
export AWS_REGION=eu-central-1
export AWS_ACCESS_KEY_ID="AKIAV7E2LCRATIHUYNWG"
export AWS_SECRET_ACCESS_KEY=changeme
export AVP_TYPE=awssecretsmanager

```

create the namespace

```bash
k create ns ecs-cs-argocd
```

create ecr pull secret

```bash
k create secret docker-registry ecr-registry --docker-server=https://<account>.dkr.ecr.<region>.amazonaws.com --docker-username=AWS --docker-password=$(aws ecr get-login-password --region <region>) -n ecs-cs-argocd
```

### Generate values.yaml

duplicate the template values-template.yaml

```bash
cp values-template.yaml values-\<name-of-the-cluster\>.yaml
```

replace the values in the new file by using sed or search and replace in your editor
because over the complete file there are sveral entries to be replaced

```yaml
hostname: clustername-where-argocd-is-deployed
managedProjects: ["gardener-projects-to-be-managed-by-argocd"]
adminClusterProject: projectname-where-argocd-cluster-is-deployed
stage: the-stage-of-the-deployment-devprod

awsRegion: aws-region
awsAccount: aws-account-id
```

### Apply

To apply the namespace must be declared twice because of the plugin.

```bash
helm template argocd . -f values-xxx.yaml -n ecs-cs-argocd | argocd-vault-plugin generate - | k apply  -n ecs-cs-argocd -f - 
```
