# Запуск простого приложения в helm

После установки нужно запустить Kubernetes. При необходимости можно изменить используемый драйвер с помощью
флага `--driver`.

```shell script
minikube start \
--cpus=4 --memory=8g \
--cni=flannel \
--kubernetes-version="v1.19.0" \
--extra-config=apiserver.enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,\
DefaultTolerationSeconds,NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota,PodPreset \
--extra-config=apiserver.authorization-mode=Node,RBAC
```


Создаём наймспейс
```bash
kubectl create namespace userauth
```

Ичпользуем наймспейс поумолчанию
```bash
kubectl config set-context --current --namespace=userauth
```

Установить prometheus
```bash
helm install prom prometheus-community/kube-prometheus-stack -f microarch-chart/prometheus.yaml --atomic
```

Устанавливаем через хелм ингресс
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx ingress-nginx/ingress-nginx -f microarch-chart/nginx-ingress.yaml --atomic
```

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
kubectl port-forward -n default service/prom-kube-prometheus-stack-prometheus 9090
```

Установить зависимости
```bash
helm dependency update ./userapp/microarch-chart
```

Запустить чарт userapp
```bash
helm install myapp ./userapp/microarch-chart
```