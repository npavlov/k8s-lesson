# K8s  уроки

### С чего начать

Список команд minikube

https://minikube.sigs.k8s.io/docs/commands/

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

Например, для развертывания Pods 
мы будем использовать команду `kubectl apply -f ./pods/pod-file.yml`

Для проброса порта с хостовой машины на порт внутри pod мы используем команду `kubectl port-forward my-web 8888:80`

Где my-web имя пода, 8888 - внешний порт на хосте и 80 внутренний порт на поде

### Создание нового Deployment

Разворачиваем новый deployment из файла, 1 под

`kubectl apply -f ./deployments/simple-deployment.yml`

Разворачиваем новый deployment из файла, c репликами, 3 пода

`kubectl apply -f ./deployments/simple-replicas.yml`

`replicas: 3` регулируем через этот параметр

Разворачиваем новый deployment с HPA (HorizontalPodAutoScale)

`kubectl apply -f ./deployments/simple-autoscaling.yml`

AutoScaling реализуется через вторую часть файла, разворачивается HPA

Минимум 2 pods, максимум 6, утилизация (барьер) CPU - 70%, RAM - 80%

Команды

`kubectl get rc` - проверить наличие Replication Controller и настройки с которыми задана репликация

`kubectl get hpa` - Показать все HPA - HorizontalPodAutoScalers

`kubectl create deployment ci-app-test-deployment --image httpd:latest`	Создать Deployment из DockerImage httpd:latest

`kubectl describe deplyoments ci-app-test-deployment`	Показать все данные о Deployments denis-deployment

`kubectl scale deployment ci-app-test-deployment --replicas 4`	Создать ReplicaSets

`kubectl autoscale deployment ci-app-test-deployment  --min=10 --max=15 --cpu-percent=80`	Создать AutoScaling для Deployment denis

`kubectl get hpa`	Показать все HPA - HorizontalAutoScalers

`kubectl set image deployment/ci-app-test-deployment k8sphp=adv4000/k8sphp:version2 --record`	Заменить Deployment denis-deployment Image на новый

`kubectl rollout status deployment/ci-app-test-deployment` 	Показать статус Обновления

`kubectl rollout history deployment/ci-app-test-deployment`	Показать историю Обновлении

`kubectl rollout undo deployment/ci-app-test-deployment` 	Вернуться на предидущую версию

`kubectl rollout undo deployment/ci-app-test-deployment --to-revision=2`	Вернуться на указанную версию

`kubectl rollout restart deployment/ci-app-test-deployment` 	Передеплоить текущую версию (скачивает заново DockerImage с тегом latest)

`kubectl delete deployments ci-app-test-deployment` 	Стереть Deployment denis-deployment 

### Создание нового Service

`kubectl expose deployment ci-test-deployment-autoscaled --type=LoadBalancer --port=80`

где ci-test-deployment-autoscaled - имя Deployment
--type=LoadBalancer - тип Service, в данном случае LoadBalancer
--port=80 - порт к которому будет обращаться в подах

Для типа LoadBalancer нужно выполнить команды
`minikube addons enable ingress` - включает ingress
`minikube tunnel` - пробивает туннель

`➜ kubectl apply -f ./services/service-loadbalancer-two-images.yml` - развертывание Service + HPA + Deployment

### Ingress Controller

Что делаем для старта:

`minikube addons enable ingress` - включает ingress

`kubectl get pods -n kube-system | grep nginx-ingress-controller` - проверяем что Ingress контроллер запустился

`kubectl apply -f ./ingress/ingress-hosts.yml` - запускаем Deployment, создаем ClusterIP и Ingress Controller   

`kubectl get ingress` - получаем Ingress instance

### Helm Chart

`minikube start --nodes 2` - создаем инстанс K8s

`minikube addons enable ingress` - включаем ingress

`minikube addons enable ingress-dns` - включаем ingress dns

`minikube addons list` - проверяем какие аддоны мы включили

`helm install app helm-chart` - устанавливаем наш helm chart на K8s

`helm list` - проверяем список deployed Helm Chart

`kubectl get pods` - проверяем что создались Pods

`kubectl get svc` - проверяем что создались Service по балансированию наших приложений внутри Pods

`kubectl get ingress` - проверяем что создался Ingress instance

`kubectl describe ingress ingress-hosts` - проверяем статус его развертывания

`kubectl get hpa` - проверяем что создался Horizontal Pod AutoScales

`helm upgrade app helm-chart` - для обновления нашего Helm Deployment
