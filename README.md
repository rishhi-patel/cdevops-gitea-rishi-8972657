# cdevops-gitea

k8s gitea lab to take dev (sqlite based) to prod (mysql based)


## Deployment Steps 
### 1. Set Up Kubernetes Cluster with k3d

```bash
pip install ansible kubernetes
# Clean up any previous submodule and cluster because sometime its not initlize correctly in codespaces
rm -rf k8s
git submodule deinit -f k8s


# Add the Kubernetes Ansible submodule
git submodule add https://github.com/rhildred/ansible-k8s.git k8s

ansible-playbook up.yml
# Create a new k3d cluster named 'gitea-cluster'
k3d cluster create gitea-cluster

# Set KUBECONFIG environment variable for kubectl access
export KUBECONFIG=$(k3d kubeconfig write gitea-cluster)
```

- This will initialize a local Kubernetes cluster using [k3d](https://k3d.io/) and prepare your environment for deploying MySQL and Gitea.
- Make sure you have [k3d](https://k3d.io/) and [kubectl](https://kubernetes.io/docs/tasks/tools/) installed before running these commands.

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

https://cf47315699dc.ngrok-free.app

```bash
ansible-playbook ngrok/up.yml

#or
ngrok http 3000
```
### ngrok : git-tea : https://cf47315699dc.ngrok-free.app
### live : https://gitea.exotrend.live

<img width="2560" height="1600" alt="image" src="https://github.com/user-attachments/assets/18991f8d-985c-460e-a68f-2dad7e9810d0" />

<img width="2560" height="1600" alt="image" src="https://github.com/user-attachments/assets/da60437a-493b-4b6c-9507-6b2d618e1a93" />




