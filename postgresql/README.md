PostgreSQL (primary + replica)

Deploys a primary and optional replicas using the Bitnami PostgreSQL image with streaming replication. Provides two services: `postgresql-primary` (writes) and `postgresql-read` (reads).

Install

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Scale

```bash
# replicas = 1 primary + N replicas
helm upgrade --install postgresql . -n sellio-data -f ../envs/prod/values-postgresql.yaml --set replicaCount=2
```

Endpoints

- Primary: ClusterIP `postgresql-primary.sellio-data.svc:5432`
- Read:    ClusterIP `postgresql-read.sellio-data.svc:5432`

Uninstall

```bash
make uninstall ENV=prod
```


