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