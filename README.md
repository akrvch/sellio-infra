### 🚀 Sellio Infra (тільки прод, зручно для Minikube)

### ⚡ Швидкий старт

```bash
minikube start --cpus=4 --memory=8192

# Створити namespace
kubectl apply -f common/namespace.yaml

# Встановити сервіси (рекомендований порядок)
make -C kafka add-repos deps install ENV=prod
make -C kafka-connect add-repos deps install ENV=prod
make -C postgresql add-repos deps install ENV=prod
make -C redis add-repos deps install ENV=prod
make -C elasticsearch add-repos deps install ENV=prod
```

### 📝 Примітки

- Namespace: `sellio-infra`.
- Усі чарти мають NetworkPolicy (ingress з `sellio-infra` та будь-якого ns з міткою `namePrefix=sellio`), PDB та опційний ServiceMonitor (`monitoring.enabled=false` за замовчуванням).
- Ціль `diff` використовує `helm diff`, якщо доступно; інакше — `helm upgrade --install --dry-run --debug`.

### 🌐 Зовнішній доступ (minikube)

```bash
# Увімкнути ingress-контролер
minikube addons enable ingress

# Додати запис для Elasticsearch (Ingress)
# /etc/hosts
127.0.0.1 es.sellio.local

# IP Minikube
MINI_IP=$(minikube ip)
echo $MINI_IP

# 🔌 Доступ через NodePort

# PostgreSQL (primary і replica)
psql "postgresql://user:pass@${MINI_IP}:30432/main"
psql "postgresql://user:pass@${MINI_IP}:30433/main"   # replica (read-only)

# Redis
redis-cli -h "${MINI_IP}" -p 30637

# Kafka (PLAINTEXT)
kafka-console-producer --bootstrap-server "${MINI_IP}:30992" --topic test
kafka-console-consumer  --bootstrap-server "${MINI_IP}:30992" --topic test --from-beginning

# Kafka Connect (NodePort)
curl "http://${MINI_IP}:30883/connectors"

# Elasticsearch (через Ingress, без порту)
curl "http://es.sellio.local/_cat/health?v"
```

### 🏷️ Хостнейми (опційно)

```bash
# Прив’язати зручні хостнейми до IP Minikube (macOS приклад)
MINI_IP=$(minikube ip)
sudo sh -c 'sed -i "" "/sellio.local/d" /etc/hosts; echo '"$MINI_IP"' \
  kafka.sellio.local \
  redis.sellio.local \
  postgresql-primary.sellio.local \
  postgresql-replica.sellio.local \
  kafka-connect.sellio.local \
  es.sellio.local >> /etc/hosts'

# Використання хостнеймів із NodePort (TCP)
psql "postgresql://user:pass@postgresql-primary.sellio.local:30432/main"
redis-cli -h redis.sellio.local -p 30637
kafka-console-producer --bootstrap-server "kafka.sellio.local:30992" --topic test

# HTTP через Ingress (без порту)
curl "http://kafka-connect.sellio.local/connectors"
curl "http://es.sellio.local/_cat/health?v"
```

### 🧭 DNS-імена сервісів у кластері (без змін)

- postgresql-primary:5432
- postgresql-replica:5432
- redis:6379
- kafka:9092
- elasticsearch:9200
- kafka-connect:8083

Hostnames (optional)

```bash
# Map convenient hostnames to your Minikube IP (macOS example)
MINI_IP=$(minikube ip)
sudo sh -c 'sed -i "" "/sellio.local/d" /etc/hosts; echo '"$MINI_IP"' \
  kafka.sellio.local \
  redis.sellio.local \
  postgresql-primary.sellio.local \
  postgresql-replica.sellio.local \
  kafka-connect.sellio.local \
  es.sellio.local >> /etc/hosts'

# Then use hostnames with the same ports for TCP services
psql "postgresql://user:pass@postgresql-primary.sellio.local:30432/main"
redis-cli -h redis.sellio.local -p 30637
kafka-console-producer --bootstrap-server "kafka.sellio.local:30992" --topic test

# HTTP services via Ingress (no port):
curl "http://kafka-connect.sellio.local/connectors"
curl "http://es.sellio.local/_cat/health?v"
```


