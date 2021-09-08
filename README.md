# K8s  уроки

### С чего начать

Ставим minikube

`brew install minikube`

Разворачиваем minikube

`minikube start # for one node cluster
minikube start --nodes 2 -p multinode-demo # for two nodes cluster`

--nodes - количество нод для кластера

Проверяем что создается deployment и выполняем expose 80ого порта

`kubectl create deployment hello-minikube --image=docker/getting-started --replicas=2
kubectl expose deployment hello-minikube --type=NodePort --port=80`

`kubectl get nodes` - проверка количества поднятых nodes

Для просмотра deployments используем

`kubectl get deployments`

Получим статус по сервису и информацию о подах и сервисах

`kubectl rollout status deployment/hello-minikube
kubectl get rs,pods,service`

Попытаемся получить доступ к сервису

`minikube service hello-minikube`

Команда вернет нам адрес по которому будет доступен pod

## Из чего состоит этот репозиторий

Репозиторий состоит из файлов для уроков по изучение K8s

### Создание нового Pod

Например для развертывания Pods 
мы будем использовать команду `kubectl apply -f ./pods/pod-file.yml`

Для проброса порта с хостовой машина на порт внутри pod мы используем команду `kubectl port-forward my-web 8888:80`

Где my-web имя пода, 8888 - внешний порт на хосте и 80 внутренний порт на поде

### Создание нового Deployment

Команды

`kubectl get rc` - проверить наличие Replication Controller и настройки с которыми задана репликация

`kubectl get hpa` - Показать все HPA - HorizontalPodAutoScalers

`kubectl create deployment denis-deployment --image httpd:latest`	Создать Deployment из DockerImage httpd:latest

`kubectl describe deplyoments denis-deployment`	Показать все данные о Deployments denis-deployment

`kubectl scale deployment denis-deployment --replicas 4`	Создать ReplicaSets

`kubectl autoscale deployment denis  --min=10 --max=15 --cpu-percent=80`	Создать AutoScaling для Deployment denis

`kubectl get hpa`	Показать все HPA - HorizontalAutoScalers

`kubectl set image deployment/denis-deployment k8sphp=adv4000/k8sphp:version2 --record`	Заменить Deployment denis-deployment Image на новый

`kubectl rollout status deployment/denis-deployment` 	Показать статус Обновления

`kubectl rollout history deployment/denis-deployment`	Показать историю Обновлении

`kubectl rollout undo deployment/denis-deployment` 	Вернуться на предидущую версию

`kubectl rollout undo deployment/denis-deployment --to-revision=2`	Вернуться на указанную версию

`kubectl rollout restart deployment/denis-deployment` 	Передеплоить текущую версию (скачивает заново DockerImage с тегом latest)

`kubectl delete deployments denis-deployment` 	Стереть Deployment denis-deployment 


