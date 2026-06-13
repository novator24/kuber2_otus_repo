# Репозиторий для выполнения домашних заданий курса "Инфраструктурная платформа на основе Kubernetes-2026-05" 


## Useful commands

### 1. kubernetes-into

```bash
kubectl create -f ./kubernetes-intro/namespace.yaml
```

```bash
kubectl config set-context --current --namespace=homework
```

```bash
kubectl config view --minify | grep namespace
```

```bash
kubectl apply -f ./kubernetes-intro/pod.yaml
```


```bash
kubectl get pods
```

```bash
kubectl describe pod angie
```

```bash
kubectl logs angie
```

```bash
kubectl exec -it angie -- sh
```

```bash
kubectl delete pod angie -n homework
kubectl apply -f ./kubernetes-intro/pod.yaml
```

```bash
kubectl exec -n homework angie -- angie -T
```

### 2. kubernetes-controlers

```bash
kubectl get nodes --show-labels
```

```bash
kubectl label node minikube homework=true
```

```bash
kubectl apply -f ./kubernetes-controllers
```

```bash
kubectl rollout status deployment angie-deployment
```

```bash
kubectl get pods -l app=angie -w
```

```bash
kubectl get deployment angie-deployment -o yaml | grep -A5 strategy
```


```bash
kubectl describe deployment angie-deployment
```

### 3. kubernetes-networks

```bash
kubectl get svc -n homework
```

```bash
kubectl get endpoints angie-service -n homework
```

```bash
kubectl describe svc angie-service -n homework
```

```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
```

```bash
helm uninstall traefik
```


```bash
helm install traefik traefik/traefik --set providers.kubernetesGateway.enabled=true  --namespace traefik --create-namespace
```


```
ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ helm install traefik traefik/traefik   --namespace traefik   --create-namespace
NAME: traefik
LAST DEPLOYED: Fri Jun 12 16:58:53 2026
NAMESPACE: traefik
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
traefik with docker.io/traefik:v3.7.4 has been deployed successfully on traefik namespace!
```

```bash
helm list -A
```


```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.4.0/standard-install.yaml
```


```bash
kubectl get crd | grep gateway
```


```bash
kubectl get gatewayclass
```

