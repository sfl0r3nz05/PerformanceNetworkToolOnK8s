# IPerf3

IPerf3 is built on a `client-server` model and measures maximum User Datagram Protocol, TCP and Stream Control Transmission Protocol throughput between client and server stations. It can also be used to measure LAN and wireless LAN throughput.

## Pre-requisites

- Have a K8s/minikube cluster installed

## Install IPerf3 server on K8s cluster based on helm chart

1. Clone the IPerf repository

   ```console
   git clone https://eugenmayer.github.io/helm-charts/
   ```

2. Go to Iperf3 repository

   ```console
   cd ~/helm-charts/charts
   ```

3. Install IPerf3 helm chart

   ```console
   helm install my-iperf3 iperf3/ --version 0.2.0 -n iperf3
   ```

    - Expected Output:

    ```console
    NAME                          READY   STATUS    RESTARTS   AGE
    pod/iperf3-6b779c8957-2hkxz   1/1     Running   0          27m

    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE
    service/iperf3   LoadBalancer   10.152.183.206   10.5.0.101    40000:30661/TCP   27m

    NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/iperf3   1/1     1            1           27m

    NAME                                DESIRED   CURRENT   READY   AGE
    replicaset.apps/iperf3-6b779c8957   1         1         1       27m
    ```

4. Package helm chart

    > Only if necessary: *Package IPerf3 to then export it*.

   ```console
   helm package iperf3/
   ```

    - Expected Output:

    ```console
    iperf3-0.2.1.tgz
    ```

    > This file is included into this repository in the directory: `~/PerformanceNetworkToolsOnK8s/Iperf3/iperf3-0.2.1.tgz`

