Elasticsearch (single node)

Deploys a single-node Elasticsearch with a headless service for discovery and a client service on port 9200.

Install

```bash
kubectl apply -f ../common/namespace.yaml
make add-repos deps install ENV=prod
```

Endpoints

- Client: ClusterIP `elasticsearch.sellio-data.svc:9200`

Scale

```bash
helm upgrade --install elasticsearch . -n sellio-data -f ../envs/prod/values-elasticsearch.yaml --set replicas=1
```

Uninstall

```bash
make uninstall ENV=prod
```


