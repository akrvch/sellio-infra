Kafka Connect (один вузол)

Розгортає одновузловий Kafka Connect із двома PVC: `plugins` і `buffer`. Приклад конфігурації конектора в `configs/example-connector.json`.

Встановлення

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Точка доступу

- REST: ClusterIP `kafka-connect.sellio-infra.svc:8083`

Додати конектор (вручну)

```bash
kubectl -n sellio-infra port-forward svc/kafka-connect 8083:8083 &
curl -X POST -H 'Content-Type: application/json' \
  --data @configs/example-connector.json \
  http://localhost:8083/connectors
```

Видалення

```bash
make uninstall ENV=prod
```


