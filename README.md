# 🚀 Spark on Kubernetes — локальное изучение

Проект демонстрирует запуск Apache Spark приложения в Kubernetes-кластере, поднятом локально через Docker Desktop.

---

## 📦 Стек технологий

- Apache Spark 3.x
- Kubernetes (Docker Desktop)
- Java 17
- Docker
- kubectl
- Spark UI

---

## 🎯 Цель проекта

Изучить полный цикл работы Spark в Kubernetes:

- Подготовка окружения
- Сборка Docker образа Spark приложения
- Запуск Spark job в Kubernetes
- Мониторинг через Spark UI

---

## 🧱 Подготовка окружения

### 1. Включить Kubernetes в Docker Desktop

Убедитесь, что кластер запущен:

```bash
kubectl cluster-info
```

Пример вывода:
Kubernetes control plane is running at https://127.0.0.1:6443
CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

### 2. Установка Java 17

```bash
brew install openjdk@17
```

### 3. Настройка переменных окружения

```text
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH=$JAVA_HOME/bin:$PATH
```

Проверка:
```bash
java -version
```

## 🐳 Сборка Spark приложения

Создание Docker image с Spark application:
```bash
docker build -t my-spark-app:1.0 .
```
### ⚙️ Запуск Spark в Kubernetes
```bash
spark-submit \
  --master k8s://https://127.0.0.1:6443 \
  --deploy-mode cluster \
  --name k8s-test \
  --conf spark.kubernetes.namespace=spark \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
  --conf spark.kubernetes.container.image=my-spark-app:1.0 \
  local:///opt/spark/work-dir/app.py
```

## 📊 Spark UI (мониторинг job)

### 1. Найти driver pod
```bash
kubectl get pods -n spark
```
### 2. Пробросить порт Spark UI
```bash
kubectl port-forward -n spark pod/k8s-test-<driver-id> 4040:4040
```

### 3. Открыть UI в браузере
```text
http://127.0.0.1:4040
```

## 📌 Итог

Проект позволяет понять:
- как Spark запускается в Kubernetes
- как работает driver/executor модель
- как устроен Spark UI
- как собирать distributed data pipeline локально