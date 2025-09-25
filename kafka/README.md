Kafka (Redpanda, один вузол)

Розгортає один вузол Redpanda з Kafka API на 9092, а також post-install job для створення топіків.

Встановлення

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Топіки

Визначені у `../envs/prod/values-kafka.yaml`. Приклад:

```yaml
topics:
  - name: sellio.events.notifications
    partitions: 1
    replicationFactor: 1
```

Точки доступу

- Kafka: ClusterIP `kafka.sellio-infra.svc:9092`

Видалення

```bash
make uninstall ENV=prod
```


