Kafka Connect (single node)

Deploys a single-node Kafka Connect with two PVCs: `plugins` and `buffer`. Example connector config provided in `configs/example-connector.json`.

Install

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Endpoint

- REST: ClusterIP `kafka-connect.sellio-data.svc:8083`

Apply a connector (manual)

```bash
kubectl -n sellio-data port-forward svc/kafka-connect 8083:8083 &
curl -X POST -H 'Content-Type: application/json' \
  --data @configs/example-connector.json \
  http://localhost:8083/connectors
```

Uninstall

```bash
make uninstall ENV=prod
```


