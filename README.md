<div align="center">

<h1>KubeCoin Helm Charts</h1>

<p><strong>Helm-based deployment for frontend, backend, and PostgreSQL primary/replica with dynamic storage</strong></p>

![Helm](https://img.shields.io/badge/Helm-v3-0ea5e9?style=for-the-badge)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployments%20%2B%20StatefulSets-2563eb?style=for-the-badge)
![Storage](https://img.shields.io/badge/PV%2FPVC-Dynamic%20Provisioning-1d4ed8?style=for-the-badge)
![Learning](https://img.shields.io/badge/Learning-yamls%2F%20Included-4338ca?style=for-the-badge)

</div>

---

## Contents

| Path | Purpose |
|---|---|
| `kubecoin/` | Helm chart templates and values |
| `yamls/` | Plain Kubernetes manifests for study/experiments |

## Quick Start

```bash
helm upgrade --install kubecoin ./kubecoin -n kubecoin --create-namespace
kubectl get all,pvc,pv -n kubecoin
```

## Study Mode

Apply plain manifests without Helm:

```bash
kubectl apply -f yamls/
```

Use this when you want to learn each resource directly.

## CI/CD Note

- Jenkins pipelines build and push Docker images using the Jenkins credential `docker-creds`.
- Helm image values are updated from CI and pushed back to Git using `git-creds`.
- Image updates target `kubecoin-helm-charts/kubecoin/values.yaml`.
