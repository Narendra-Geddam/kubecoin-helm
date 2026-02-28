# Kubecoin Helm Chart

This chart packages the Kubernetes manifests from `k8s/dev` into Helm templates.

## Namespace

This chart includes a namespace manifest (`templates/namespace.yaml`).
- `namespace.create=true`: chart creates a Namespace.
- `namespace.name=""`: uses `.Release.Namespace` from `helm install -n ...`.

## Install

```bash
helm install kubecoin . -n dev
```

## PostgreSQL StatefulSet + Dynamic PV

The database workload is deployed as a `StatefulSet` and uses `volumeClaimTemplates`.
With a dynamic StorageClass (for example `local-path`), Kubernetes will create a PVC/PV for each DB pod.

Default persistence values:

- `database.persistence.enabled=true`
- `database.persistence.storageClass=local-path`
- `database.persistence.accessModes=[ReadWriteOnce]`
- `database.persistence.size=8Gi`

Verify after install/upgrade:

```bash
kubectl get statefulset,po,pvc,pv -n dev
```

## Scale

Scale app pods to 2 replicas:

```bash
helm upgrade kubecoin . -n dev \
  --set backend.replicas=2 \
  --set frontend.replicas=2
```

PostgreSQL replication is not configured in this chart. Keep `database.replicas=1` unless you add a replication setup.

## Upgrade

```bash
helm upgrade kubecoin . -n dev
```

## Uninstall

```bash
helm uninstall kubecoin -n dev
```
