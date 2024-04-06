# tst-datalakehouse-hudi



# Create cluster
kind create cluster --config ./kind/cluster.yaml

# Delete cluster
kind delete cluster --name datakube-lakehouse

# Ingress Controller - Nginx
kubectl apply -f k8s/nginx/nginx.yaml

# Helm - Repos
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Prometheus
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack --values k8s/prometheus/operator.yaml --version 57.2.0 --namespace monitoring --create-namespace