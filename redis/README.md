Redis (3-node masters)

Deploys a lightweight Redis StatefulSet with 3 pods (if `cluster.enabled=true`), a headless service and an entry service `redis-entrypoint` for applications.

Install

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Endpoints

- Entry: ClusterIP `redis-entrypoint.sellio-infra.svc:6379`

Uninstall

```bash
make uninstall ENV=prod
```


