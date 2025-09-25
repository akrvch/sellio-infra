Redis (3 вузли, masters)

Розгортає легкий Redis StatefulSet з 3 подами (якщо `cluster.enabled=true`), headless сервіс та entry-сервіс `redis-entrypoint` для застосунків.

Встановлення

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Точки доступу

- Entry: ClusterIP `redis-entrypoint.sellio-infra.svc:6379`

Видалення

```bash
make uninstall ENV=prod
```


