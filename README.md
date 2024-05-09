# tst-datalakehouse-hudi



# Create cluster
kind create cluster --config ./kind/cluster.yaml

# Delete cluster
kind delete cluster --name datakube-lakehouse

# Ingress Controller - Nginx
kubectl apply -f k8s/nginx/nginx.yaml

# Helm - Repos
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add strimzi https://strimzi.io/charts
helm repo add kafka-ui https://provectus.github.io/kafka-ui-charts
helm repo add minio-operator https://operator.min.io
helm repo add cloudnative-pg https://cloudnative-pg.io/charts/
helm repo update

# Prometheus
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack --values k8s/prometheus/operator.yaml --version 57.2.0 --namespace monitoring --create-namespace

# Strimzi
helm upgrade --install strimzi-operator strimzi/strimzi-kafka-operator --values k8s/strimzi/operator.yaml --version 0.40.0 --namespace strimzi --create-namespace
kubectl apply -f k8s/strimzi/kafka/kafka.yaml

# Kafka UI
helm upgrade --install kafka-ui kafka-ui/kafka-ui --values k8s/kafka-ui/kafka-ui.yaml --version 0.7.6 --namespace kafka-ui --create-namespace

# Data Generator
kubectl create namespace data
kubectl apply -f k8s/data-generator/deployment.yaml

# Minio
helm upgrade --install minio-operator minio-operator/operator --values k8s/minio/operator.yaml --version 5.0.14 --namespace minio --create-namespace
helm upgrade --install minio-tenant minio-operator/tenant --values k8s/minio/tenant.yaml --version 5.0.14 --namespace minio --create-namespace

# CloudNative PG
helm upgrade --install cnpg-operator  cloudnative-pg/cloudnative-pg --values k8s/cloudnative-pg/operator.yaml --version 0.21.0 --namespace cnpg --create-namespace
helm upgrade --install cnpg-postgresql  cloudnative-pg/cluster --values k8s/cloudnative-pg/postgresql.yaml --version 0.0.8 --namespace cnpg --create-namespace

