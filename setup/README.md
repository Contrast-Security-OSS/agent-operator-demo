# Cluster Setup

## Install K3d

```bash
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

```bash
% k3d --version
k3d version v5.4.6
k3s version v1.24.4-k3s1 (default)
```

## Create the Cluster

```bash
# Create the cluster.
k3d cluster create \
    contrast-agent-operator-demo \
    --kubeconfig-switch-context \
    --api-port 6550 \
    -p "8082:30082@loadbalancer" \
    -p "8083:30083@loadbalancer" \
    -p "8084:30084@loadbalancer" \
    --agents 3

# Bootstrap ArgoCD.
kubectl apply -k src/workloads/argocd

# Added ArgoCD projects (to start loading everything else).
kubectl apply -f src/projects

# Make sure to add the required secrets.
kubectl -n contrast-agent-operator \
    create secret generic default-agent-connection-secret \
    --from-literal=apiKey=TODO \
    --from-literal=serviceKey=TODO \
    --from-literal=userName=TODO
```

## Cleanup

```bash
k3d cluster delete contrast-agent-operator-demo
```
