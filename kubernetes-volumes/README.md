### Выполнено ДЗ № 4
###  Основное ДЗ
###  Задание со *
### В процессе сделано:
### Конфиг мап из прошлых ДЗ переименован в cm.yaml
### Создан объект типа storageClass с provisioner https://k8s.io/minikube-hostpath и reclaimPolicy Retain
### Создан PVC на основе storageClass из прошлого пункта.
### Перефоматирована deployment.yaml под использование SVC
### Как запустить проект:
### minikube delete && minikube start
### cd kubernetes-volumes
### kubectl config set-context --current --namespace=homework
### kubectl apply -f namespace.yaml && kubectl apply -f cm.yaml && kubectl apply -f storageClass.yaml && kubectl apply -f pvc.yaml && kubectl apply -f deployment.yaml
### Как проверить работоспособность:
### Чек подов:
### kubectl get pods
### 
### NAME                                READY   STATUS    RESTARTS   AGE
### angie-deployment-6f9d677887-fvppv   1/1     Running   0          11m
### angie-deployment-6f9d677887-qv5r6   1/1     Running   0          11m
### angie-deployment-6f9d677887-tg742   1/1     Running   0          11m
### Проверка монтирования конфиг-мапа и его доступность по УРЛУ:
### 
### kubectl exec -it angie-deployment-6f9d677887-fvppv -- sh -c "wget -qO- http://127.0.0.1:8000/conf/angie-default.conf"
### Вывод:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl exec -it angie-deployment-6f9d677887-fvppv -- sh -c "wget -qO- http://127.0.0.1:8000/conf/angie-default.conf"
### Defaulted container "nginx-fork" out of: nginx-fork, wget-index-html (init)
### server {
###     listen 8000;
### 
###     root /homework;
###     index index.html;
### 
###     location / {
###         try_files $uri $uri/ =404;
###     }
###     
### }
### Пишем в PVC:
### 
### kubectl exec -it angie-deployment-74789cf9f-7mlhk -- sh -c "echo 'The new episode 8 of From airs on June 14, 2026.' > /homework/test-pvc"
### Удаляем под:
### 
### kubectl delete pod angie-deployment-74789cf9f-7mlhk
### pod "angie-deployment-74789cf9f-7mlhk" deleted from homework namespace
### Проверяем на новом:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl get pods
### NAME                               READY   STATUS    RESTARTS   AGE
### angie-deployment-74789cf9f-jr999   1/1     Running   0          14m
### angie-deployment-74789cf9f-kmldh   1/1     Running   0          16s
### angie-deployment-74789cf9f-mhrks   1/1     Running   0          14m
### Все на месте:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl exec -it angie-deployment-74789cf9f-kmldh -- sh -c "cat /homework/test-pvc"
### Defaulted container "nginx-fork" out of: nginx-fork, wget-index-html (init)
### The new episode 8 of From airs on June 14, 2026.
### Удаляем деплоймент:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl delete deployment angie-deployment
### deployment.apps "angie-deployment" deleted from homework namespace
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl get deployments
### No resources found in homework namespace.
### Смотрим PVC:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl get pvc
### NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        VOLUMEATTRIBUTESCLASS   AGE
### homework-pvc   Bound    pvc-b5b9fadd-9cfe-416f-bb9a-2d0381f685b6   1Gi        RWO            homework-hostpath   <unset>                 3m53s
### Создаем снова деплоймент:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl apply -f deployment.yaml 
### deployment.apps/angie-deployment created
### Смотрим поды:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$ kubectl get pods
### NAME                               READY   STATUS    RESTARTS   AGE
### angie-deployment-74789cf9f-42nk2   1/1     Running   0          16s
### angie-deployment-74789cf9f-62xzr   1/1     Running   0          16s
### angie-deployment-74789cf9f-gsknl   1/1     Running   0          16s
### Проверяем раннее созданный файл:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-volumes$  kubectl exec -it angie-deployment-74789cf9f-62xzr -- sh -c "cat /homework/test-pvc"
### Defaulted container "nginx-fork" out of: nginx-fork, wget-index-html (init)
### The new episode 8 of From airs on June 14, 2026.
### PR checklist:
###  Выставлен label с темой домашнего задания
