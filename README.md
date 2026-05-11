# 📌 Изучение работы Spark в связке с k8s.

## 💻 Что в проекте
Последовательность действий для запуска Spark приложения в k8s.
- ✔️ Настройка k8s в Docker.
- ✔️ Установка Java.
- ✔️ Deploy Spark приложения в Docker.
- ✔️ Запуск приложения в k8s.
- ✔️ Анализ Spark UI.

## 📖 Описание
Последовательность запуска проекта локально:
- ✔️ Включить Cluster k8s в Docker.
- ✔️ Установить Java 17 на локальную машину.
     ```bash
     brew install openjdk@17
     ```
- ✔️ Добавить переменную окружения
     ```text
     export JAVA_HOME=$(/usr/libexec/java_home -v 17)
     export PATH=$JAVA_HOME/bin:$PATH
     ```
- ✔️ Получить информацию о k8s:
     ```
     kubectl cluster-info
     ```
     - Kubernetes control plane is running at https://127.0.0.1:6443
     - CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
- ✔️ Создать Docker image, который будет содержать файл с application.
     ```bash
     docker build -t my-spark-app:1.0 .
     ```
- ✔️ Запуск Spark приложения:
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
- ✔️ Можно посмотреть Spark UI:
     ```
     kubectl get pods -n spark
     kubectl port-forward -n spark pod/k8s-test-aecfb39e16d08287-driver 4040:4040
     
     http://127.0.0.1:4040/
     ```