# IPerf3

IPerf3 is built on a `client-server` model and measures maximum User Datagram Protocol, TCP and Stream Control Transmission Protocol throughput between client and server stations. It can also be used to measure LAN and wireless LAN throughput.

Server Client Concept for Testing `IPerf`.

- **Server:** The data receiving node is referred as Server.
- **Client:** The data sending node is referred as Client.

![image](https://user-images.githubusercontent.com/6643905/214623102-f2a7ed12-e602-4f6a-a9fc-afc49f7b6ef9.png)

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

3. Create namespace

   ```console
   kubectl create namespace iperf3
   ```

4. Install IPerf3 helm chart

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

5. Package helm chart

    > Only if necessary: *Package IPerf3 to then export it*.

   ```console
   helm package iperf3/
   ```

    - Expected Output:

    ```console
    iperf3-0.2.1.tgz
    ```

    > This file is included into this repository in the directory: `~/PerformanceNetworkToolsOnK8s/Iperf3/iperf3-0.2.1.tgz`

## Install IPerf3 client on a remote machine

1. Install `IPerf3` on a host:

   ```console
   sudo apt -y install iperf3
   ```

## Use IPerf3 to test from client to server

1. Verify deployed server:

   ```console
   kubectl logs pod/iperf3-6b779c8957-2hkxz -n iperf3
   ```

   - Output:

   ```console
   -----------------------------------------------------------
   Server listening on 40000
   -----------------------------------------------------------
   ```

2. Send frame from clients:

   ```console
   iperf3 -c 10.5.0.101 -p 40000
   ```

   ```console
   Connecting to host 10.5.0.101, port 40000
   [  5] local 10.5.0.2 port 40088 connected to 10.5.0.101 port 40000
   [ ID] Interval           Transfer     Bitrate         Retr  Cwnd
   [  5]   0.00-1.00   sec  2.84 GBytes  24.4 Gbits/sec    0   3.14 MBytes
   [  5]   1.00-2.00   sec  2.75 GBytes  23.6 Gbits/sec    0   3.14 MBytes
   [  5]   2.00-3.00   sec  2.62 GBytes  22.5 Gbits/sec    0   3.14 MBytes
   [  5]   3.00-4.00   sec  2.77 GBytes  23.8 Gbits/sec    0   3.14 MBytes
   [  5]   4.00-5.00   sec  2.69 GBytes  23.1 Gbits/sec    0   3.14 MBytes
   [  5]   5.00-6.00   sec  2.72 GBytes  23.3 Gbits/sec    0   3.14 MBytes
   [  5]   6.00-7.00   sec  2.70 GBytes  23.1 Gbits/sec    0   3.14 MBytes
   [  5]   7.00-8.00   sec  2.36 GBytes  20.3 Gbits/sec    0   3.14 MBytes
   [  5]   8.00-9.00   sec  2.23 GBytes  19.1 Gbits/sec    0   3.14 MBytes
   [  5]   9.00-10.00  sec  2.40 GBytes  20.6 Gbits/sec    0   3.14 MBytes
   - - - - - - - - - - - - - - - - - - - - - - - - -
   [ ID] Interval           Transfer     Bitrate         Retr
   [  5]   0.00-10.00  sec  26.1 GBytes  22.4 Gbits/sec    0             sender
   [  5]   0.00-10.00  sec  26.1 GBytes  22.4 Gbits/sec                  receiver

   iperf Done.
   ```
