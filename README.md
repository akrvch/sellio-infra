Sellio Infra (prod-only, minikube-friendly)

Quickstart

```bash
minikube start --cpus=4 --memory=8192

# Create namespace
kubectl apply -f common/namespace.yaml

# Install stacks (order recommended)
make -C kafka add-repos deps install ENV=prod
make -C kafka-connect add-repos deps install ENV=prod
make -C postgresql add-repos deps install ENV=prod
make -C redis add-repos deps install ENV=prod
make -C elasticsearch add-repos deps install ENV=prod
```

Notes

- Namespace is `sellio-infra`.
- All charts include NetworkPolicy (ingress from `sellio-infra` and any ns labeled with `namePrefix=sellio`), PDB, and optional ServiceMonitor (`monitoring.enabled=false` by default).
- Diff target uses `helm diff` if available; otherwise falls back to `helm upgrade --install --dry-run --debug`.


