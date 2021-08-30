# Helm Kustomize Example

This example will deploy the [Kubernetes sample guestbook](https://github.com/kubernetes/examples/tree/master/guestbook/) application as
packaged as a Helm chart downloaded from a third party source and will modify the helm chart using Kustomize.
The app will be deployed into the `fleet-helm-kustomize-example` namespace.

```yaml
kind: GitRepo
apiVersion: fleet.cattle.io/v1alpha1
metadata:
  name: helm-kustomize
  namespace: fleet-local
spec:
  repo: https://github.com/rancher/fleet-examples
  paths:
  - single-cluster/helm-kustomize
```

# Tool for local deployment

- minikube or k3s (local cluster)
- skaffold (local k8s development tool)
- docker (for build image)
- virtualbox [optional for mac user]


# Local deployment
## Tool

- minikube or k3s (local cluster)
- skaffold (local k8s development tool)
- docker (for build image)
- virtualbox [optional for mac user]

## Setup
### 1. Start cluster

```bash
minikube start -p my-profile --vm=true
```
`--vm` required for mac user when use ingress)

`-p` for create a profile for skaffold to connect


### 2. Enable ingress
```bash
minikube addons enable ingress -p my-profile
```
`-p` for specify specific profile to apply

### 3. Create image pull secret
```bash
kubectl create secret docker-registry biggestfan-registry --docker-server=registry.digitalocean.com  --docker-username=<username>  --docker-password=<password>  --docker-email=<email>
```
### 4. Config skaffold context
```bash
skaffold config set --kube-context my-profile local-cluster true
```
Tell Skaffold that the Kubernetes context my-profile refers to a local cluster

## Develop
*Do this every time you entry a directory*
### 1. Create minikube environment variables
```bash
source <(minikube docker-env -p my-profile)
```
source necessary environment variables
 ### 2. Run skaffold
 ```bash
skaffold dev
 ```


# Utility command

`minikube ip -p my-profile` Get cluster ip

`minikube delete -p my-profile` Delete cluster
