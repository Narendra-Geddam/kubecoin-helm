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

## Upgrade

```bash
helm upgrade kubecoin . -n dev
```

## Uninstall

```bash
helm uninstall kubecoin -n dev
```