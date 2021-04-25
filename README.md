# Запуск простого приложения в helm

Установить зависимости
```bash
helm dependency update ./microarch-chart
```

Запустить чарт
```bash
helm install myapp ./microarch-chart
```

Применить миграции
```bash
kubectl apply -f microarch-chart/initdb.yaml
```