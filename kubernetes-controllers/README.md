### Выполнено ДЗ № 3
###  Основное ДЗ
###  Задание со *
### В процессе сделано:
### Пункт 1
### Пункт 2
### Как запустить проект:
### Запустить инфру с прошлого ДЗ.
### minikube delete && minikube start
### cd kubernetes-networks
### kubectl apply -f namespace.yaml
### kubectl config set-context --current --namespace=homework
### kubectl apply -f ./additional/manifests/configMap.yaml
### kubectl apply -f deployment.yaml
### Внезапно, на миникубе нет нуждных CRD:
### kubectl get gatewayclass
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get gatewayclass
### error: the server doesn't have a resource type "gatewayclass"
### Установка:
### kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.5.1/standard-install.yaml
### 
### Проверяем еще раз:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get gatewayclass
### No resources found
### Теперь нужно доставить сам траефик:
### 
### helm repo add traefik https://traefik.github.io/charts && helm repo update && \
### helm install traefik traefik/traefik --namespace traefik --create-namespace -f ./additional/helm/traefic-values.yaml
### Внезапно, чтобы он отобразился на ГВ нужно дополнительно задавать параметры установки ( описано в traefik-values.yaml
### 
### Вывод:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ helm repo add traefik https://traefik.github.io/charts && helm repo update && \
### helm install traefik traefik/traefik --namespace traefik --create-namespace -f ./additional/helm/traefic-values.yaml
### "traefik" already exists with the same configuration, skipping
### Hang tight while we grab the latest from your chart repositories...
### ...Successfully got an update from the "longhorn" chart repository
### ...Successfully got an update from the "traefik" chart repository
### ...Successfully got an update from the "grafana" chart repository
### ...Successfully got an update from the "prometheus-community" chart repository
### Update Complete. ⎈Happy Helming!⎈
### NAME: traefik
### LAST DEPLOYED: Sat Jun 13 12:55:38 2026
### NAMESPACE: traefik
### STATUS: deployed
### REVISION: 1
### TEST SUITE: None
### NOTES:
### traefik with docker.io/traefik:v3.7.4 has been deployed successfully on traefik namespace!
### 
### ⚠️ DEPRECATION WARNING: Gateway API CRDs will no longer be shipped with this chart in a future major version.
### You will need to install them yourself before deploying Traefik v3.7:
###   kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.5.1/standard-install.yaml
### Работает:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get gatewayclass
### NAME      CONTROLLER                      ACCEPTED   AGE
### traefik   traefik.io/gateway-controller   True       81s
### Продолжаем доставлять компоненты:
### Гейтвей со ссылкой на gateway-class траефика:
### 
### kubectl apply -f gateway.yaml
### 
### Вывод:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl apply -f gateway.yaml 
### gateway.gateway.networking.k8s.io/homework-gateway created
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get gateway
### NAME               CLASS     ADDRESS   PROGRAMMED   AGE
### homework-gateway   traefik                          12s
### Сервис балансировки между подами:
### kubectl apply -f service.yaml
### 
### Вывод:
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl apply -f service.yaml 
### service/angie-service created
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get svc
### NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
### angie-service   ClusterIP   10.108.71.217   <none>        8000/TCP   2s
### ХТТП-роут ( с учетом задания со *, реврайт там, а не на angie):
### 
### kubectl apply -f httpRoute.yaml
### 
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl apply -f httpRoute.yaml 
### httproute.gateway.networking.k8s.io/homework-route created
### ubuntu@ubuntu-MS-7C52:~/otus/bralbral_repo/kubernetes-networks$ kubectl get httproute
### NAME             HOSTNAMES           AGE
### homework-route   ["homework.otus"]   5s
### Добавляем нужный сайт на локальной машине:
### echo "127.0.0.1 homework.otus" | sudo tee -a /etc/hosts
### curl http://homework.otus/index.html
### И ТУТ НАЧИНАЮТСЯ ПРОБЛЕМЫ:
### 
### согласно заданию, достаточно выполнить minikube tunnel
### После его выполения, в SVC должен был прописаться IP-шник машины хоста. В моем случае прописывался точно такой же IP-ник кластерный.
### 
### Подумал, может нужно от рута делать: `sudo minikube tunnel - изначально миникуб запущен без него, и нормально запутисть не дает, выдает ворнинг, касательно того, что докер пытаемся из под рута вызвать.
### 
### В итоге погуглив, нашел, что можно порт-форвард сделать явно с установленного траефика, эмулируя minikube tunnel.
### 
### kubectl port-forward -n traefik service/traefik 8000:8000
### Параметры хоста:
### 
### ubuntu@ubuntu-MS-7C52:~$ cat /etc/os-release 
### PRETTY_NAME="Ubuntu 24.04.4 LTS"
### NAME="Ubuntu"
### VERSION_ID="24.04"
### VERSION="24.04.4 LTS (Noble Numbat)"
### VERSION_CODENAME=noble
### ID=ubuntu
### ID_LIKE=debian
### HOME_URL="https://www.ubuntu.com/"
### SUPPORT_URL="https://help.ubuntu.com/"
### BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
### PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
### UBUNTU_CODENAME=noble
### LOGO=ubuntu-logo
### ubuntu@ubuntu-MS-7C52:~$ 
### Как проверить работоспособность:
### curl http://homework.otus/index.html
### curl http://homework.otus/homepage
### Или в браузере.
### 
### PR checklist:
###  Выставлен label с темой домашнего задания
