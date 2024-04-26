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
helm repo add minio-operator https://operator.min.io
helm repo update

# Prometheus
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack --values k8s/prometheus/operator.yaml --version 57.2.0 --namespace monitoring --create-namespace

# Strimzi
helm upgrade --install strimzi-operator strimzi/strimzi-kafka-operator --values k8s/strimzi/operator.yaml --version 0.40.0 --namespace strimzi --create-namespace
kubectl apply -f k8s/strimzi/kafka/kafka.yaml

# Minio
helm upgrade --install minio-operator minio-operator/operator --values k8s/minio/operator.yaml --version 5.0.14 --namespace minio --create-namespace
helm upgrade --install minio-tenant minio-operator/tenant --values k8s/minio/tenant.yaml --version 5.0.14 --namespace minio --create-namespace
