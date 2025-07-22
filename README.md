# cdevops-gitea

k8s gitea lab to take dev (sqlite based) to prod (mysql based)

## Deployment Steps

### 1. Create Kubernetes cluster with k3d

```bash
pip install ansible kubernetes
git submodule update --init --recursive
ansible-playbook up.yml

k3d cluster create gitea-cluster
export KUBECONFIG=$(k3d kubeconfig write gitea-cluster)
```

### 2. Deploy MySQL

```bash
kubectl apply -f mysql.yaml
```

### 3. Deploy Gitea

```bash
ansible-playbook gitea/up.yml
```

### 4. Check pod & service status

```bash
kubectl get pods
kubectl get svc
```

### 5. Port-forward Gitea

```bash
kubectl port-forward svc/gitea-http 3000:3000
```

### 6. Expose with ngrok

```bash
ansible-playbook ngrok/up.yml
```
