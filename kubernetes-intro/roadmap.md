
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



