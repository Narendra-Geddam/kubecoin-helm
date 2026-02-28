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

The database runs as:
- one primary StatefulSet (`postgres-master`)
- one replica StatefulSet (`postgres-replica`, created when `database.replicas > 1`)

Both use `volumeClaimTemplates` so dynamic StorageClass provisioning creates PVC/PV per DB pod.

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

Scale app + database to 2:

```bash
helm upgrade kubecoin . -n dev \
  --set backend.replicas=2 \
  --set frontend.replicas=2 \
  --set database.replicas=2
```

Service endpoints:
- `database-primary-svc`: write traffic (primary)
- `database-replica-svc`: read traffic (replicas)

## Upgrade

```bash
helm upgrade kubecoin . -n dev
```

## Uninstall

```bash
helm uninstall kubecoin -n dev
```
