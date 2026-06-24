### Выполнено ДЗ № 1
###  Основное ДЗ
###  Задание со *
### В процессе сделано:
### Выполнены задания по ссылке, а именно:
### 
### Cоздан namespace.yaml с нужным неймспейсом.
### Создан pod.yaml, который реализует:
### Формирование index.html
### Запуск запуск angie (fork-nginx) на отдачу страницы index.hmtl (пришлось менять конфиг дефолтный, для этого использовал конфиг-мап)
### Как запустить проект:
### cd kubernetes-intro
### kubectl apply -f namespace.yaml
### kubectl apply -f config-map.yaml
### - kubectl apply -f pod.yaml
### Как проверить работоспособность:
### kubectl get pods ( с учетом readiness && liveness покажет статус running)
### kubectl exec -n homework angie -- cat /homework/index.html ( отдаст содежимое страницы с гх)
### kubectl exec -n homework angie -- wget -qO- http://127.0.0.1:8000/ ( отдаст тоже самое, Очень смутило почему не отдавало по локалхосту).
### PR checklist:
###  Выставлен label с темой домашнего задания
