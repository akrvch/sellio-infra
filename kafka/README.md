Kafka (Redpanda single node)

Deploys a single Redpanda node exposing Kafka API on 9092, with a post-install job to create topics.

Install

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Topics

Defined in `../envs/prod/values-kafka.yaml`. Example:

```yaml
topics:
  - name: sellio.events.notifications
    partitions: 1
    replicationFactor: 1
```

Endpoint

- Kafka: ClusterIP `kafka.sellio-infra.svc:9092`

Uninstall

```bash
make uninstall ENV=prod
```


