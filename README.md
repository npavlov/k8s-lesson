# K8s  уроки

###С чего начать

Ставим minikube

`brew install minikube`

Разворачиваем minikube

`minikube start # for one node cluster
minikube start --nodes 2 -p multinode-demo # for two nodes cluster`

Проверяем что создается deployment и выполняем expose 80ого порта

`kubectl create deployment hello-minikube --image=docker/getting-started --replicas=2
kubectl expose deployment hello-minikube --type=NodePort --port=80`

Для просмотра deployments используем

`kubectl get deployments`

Получим статус по сервису и информацию о подах и сервисах

`kubectl rollout status deployment/hello-minikube
kubectl get rs,pods,service`

Попытаемся получить доступ к сервису

`minikube service hello-minikube`

Команда вернет нам адрес по которому будет доступен pod

###Из чего состоит этот репозиторий

Репозиторий состоит из файлов для уроков по изучение K8s

Например для развертывания Pods 
мы будем использовать команду `kubectl apply -f pod-file.yml`

Для проброса порта мы используем команду `kubectl port-forward my-web 8888:80`

Где my-web имя пода, 8888 - внешний порт на хосте и 80 внутренний порт на поде