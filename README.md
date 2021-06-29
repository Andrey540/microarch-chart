# Запуск простого приложения в helm

Архитектура решения

Логинация
![login-schema](./README.assets/login-schema.png)

Аутентификация
![auth-schema](./README.assets/auth-schema.png)

Создание пользователя
![auth-schema](./README.assets/create-user-schema.png)

Пополнение счёта
![auth-schema](./README.assets/put-money-schema.png)

Создание заказа
![auth-schema](./README.assets/create-order-schema.png)

После установки нужно запустить Kubernetes. При необходимости можно изменить используемый драйвер с помощью
флага `--driver`.

```bash
minikube start \
--cpus=4 --memory=8g \
--cni=flannel \
--kubernetes-version="v1.19.0" \
--extra-config=apiserver.enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,\
DefaultTolerationSeconds,NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota,PodPreset \
--extra-config=apiserver.authorization-mode=Node,RBAC
```

Очистить неймспейс, если был создан ранее
```bash
kubectl delete namespace order
```

Создаём наймспейс
```bash
kubectl create namespace order
```

Ичпользуем наймспейс поумолчанию
```bash
kubectl config set-context --current --namespace=order
```

Добавить необходимые рапозитории
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

Установить prometheus
```bash
helm install prom prometheus-community/kube-prometheus-stack -f microarch-chart/prometheus.yaml --atomic
```

Устанавливаем через хелм ингресс
```bash
helm install nginx ingress-nginx/ingress-nginx -f microarch-chart/nginx-ingress.yaml --atomic
```

Дождаться пока ингресс запустится

Запускаем grafana
```bash
kubectl port-forward service/prom-grafana 9000:80
```

Получить пароль
```bash
kubectl get secret prom-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Запускаем prometheus
```bash
kubectl port-forward service/prom-kube-prometheus-stack-prometheus 9090
```

Устанавливаем rabbitmq. В приложении можно отключить использование rabbitmq через параметр окружения AMQP_ENABLED.
```bash
helm install rabbitmq -f microarch-chart/rabbit.yaml bitnami/rabbitmq
```

Устанавливаем kafka. В приложении можно отключить использование kafka через параметр окружения KAFKA_ENABLED.
Если включить сразу rabbitmq и kafka, то приложения будет одновременно работать с двумя брокерами и проблем с этим не будет.
```bash
helm install kafka -f microarch-chart/kafka.yaml bitnami/kafka
```

Запустить чарт userauth
```bash
helm install userauth ./microarch-chart/userauth
```

Запустить чарт userapp
```bash
helm install userapp ./microarch-chart/userapp
```

Запустить чарт billing
```bash
helm install billing ./microarch-chart/billing
```

Запустить чарт order
```bash
helm install order ./microarch-chart/order
```

Запустить чарт notification
```bash
helm install notification ./microarch-chart/notification
```