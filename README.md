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


