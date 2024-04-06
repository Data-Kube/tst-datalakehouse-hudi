# tst-datalakehouse-hudi



# Create cluster
kind create cluster --config ./kind/cluster.yaml

# Delete cluster
kind delete cluster --name datakube-lakehouse

# Ingress Controller - Nginx
kubectl apply -f k8s/nginx/nginx.yaml