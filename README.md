# Control Plane CRD Example

Example Kubernetes CRD manifests for deploying Control Plane resources using the [Control Plane Kubernetes Operator](https://github.com/controlplane-com/k8s-operator).

## Prerequisites

- Kubernetes cluster with the Control Plane Operator installed
- Authentication configured (see [operator installation guide](https://docs.controlplane.com/guides/cli/cpln-operator))

## Contents

| File | Description |
|------|-------------|
| `gvc.yaml` | Creates a GVC named `hello-world` in `aws-eu-central-1` |
| `workload.yaml` | Creates a serverless workload running nginx |

## Usage

### Before applying

Update the `org` field in both files to match your Control Plane organization name.

### Apply directly with kubectl

```bash
kubectl apply -f gvc.yaml
kubectl apply -f workload.yaml
```

### Use with ArgoCD

Create an ArgoCD Application pointing to this repository:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: cpln-example
  namespace: argocd
spec:
  project: default
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  source:
    repoURL: https://github.com/MajidAbuRmila/cpln-crd-example.git
    path: .
    targetRevision: main
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## Resources created

Once applied, the following Control Plane resources will be created:

- **GVC**: `hello-world` deployed to `aws-eu-central-1`
- **Workload**: `hello-world` running `nginx:latest` with autoscaling (1-3 replicas)
