# KAFKA

## Pre-requisites

- Have docker installed
- Have a K8s/minikube cluster installed

## Install Kafka cluster on Minikube

1. Go to Kafka/charts repository

   ```console
   cd ~/PerformanceNetworkToolOnK8s/Kafka/charts
   ```

2. Create namespace

   ```console
   kubectl create namespace kafka
   ```

3. Update the kafka helm repository

   ```console
   cd ~/PerformanceNetworkToolOnK8s/Kafka/charts/kafka
   helm dependency update
   ```

4. Install Kafka and Zookeeper cluster from helm chart

   > **Note:** Zookeeper is included in the Kafka helmchart in step number 3.

   ```console
   helm install kafka . -n kafka
   ```

## Test kafka as client on a remote machine

1. Create a topic

   ```console   
   docker run -it --rm confluentinc/cp-kafka:latest kafka-topics --create --topic ssl-perf-test --partitions 16 --replication-factor 3 --config retention.ms=86400000 --config min.insync.replicas=2 --bootstrap-server 172.31.26.62:32721
   ```

2. Test the Producer Performance

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput -1 --num-records 3000000 --record-size 1024 --producer-props acks=all bootstrap.servers=172.31.26.62:32721
   ```

3. Test the Consumer Performance

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest  kafka-consumer-perf-test --topic ssl-perf-test --broker-list 172.31.26.62:32721 --messages 3000000
   ```
