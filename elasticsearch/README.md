Elasticsearch (один вузол)

Розгортає одновузловий Elasticsearch з headless-сервісом для discovery та клієнтським сервісом на порті 9200.

Встановлення

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Точки доступу

- Client: ClusterIP `elasticsearch.sellio-infra.svc:9200`

Масштабування

```bash
helm upgrade --install elasticsearch . -n sellio-infra -f ../envs/prod/values-elasticsearch.yaml --set replicas=1
```

Видалення

```bash
make uninstall ENV=prod
```


