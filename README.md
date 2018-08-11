# How to deploy zookeeper cluster on kubernetes

---

### Prerequisites

- Install minikube or kubernetes

Follow this guide: [Install minikube on local machine](https://kubernetes.io/docs/getting-started-guides/minikube/)

Follow this guide: [Install kubernetes by kubeadm on cluster](https://kubernetes.io/docs/getting-started-guides/kubeadm/)

- Install docker-engine

Follow this guide: [Install Docker and run hello-world](https://docs.docker.com/engine/getstarted/step_one/)

---

### Clone the git repository

- Clone the git repository and change working directory to ```kubernetes-zookeeper-cluster/```

```sh
$ git clone https://github.com/shzd1984/kubernetes-zookeeper-cluster.git

$ cd kubernetes-zookeeper-cluster/
```

---

### Deploy 3 nodes zookeeper cluster in kubernetes

- Use kubectl to deploy 3 nodes zookeeper cluster in kubernetes

```sh
$ kubectl create -f .
deployment "zk-0" created
deployment "zk-1" created
deployment "zk-2" created
service "zk-0" created
service "zk-1" created
service "zk-2" created
```


Check the pods and services:

```sh
[root@k8sMaster ~]# kubectl get pod -o wide
NAME                    READY     STATUS    RESTARTS   AGE       IP             NODE
zk-1-7c8cc9889d-xcf9l   1/1       Running   0          3d        172.18.55.2    k8snode01
zk-2-7d8f45965c-m82z4   1/1       Running   0          3d        172.18.101.3   k8snode02
zk-3-5f877bc77b-jr6r5   1/1       Running   0          3d        172.18.87.3    k8snode03
```

Verify the cluster start correct or not

```sh
bash-4.4# zkCli.sh -timeout 5000 -server zk-1:2181
Connecting to zk-1:2181
2018-08-11 09:48:58,775 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.13-2d71af4dbe22557fda74f9a9b4309b15a7487f03, built on 06/29/2018 04:05 GMT
2018-08-11 09:48:58,780 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=zk-1
2018-08-11 09:48:58,781 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.8.0_171
2018-08-11 09:48:58,786 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2018-08-11 09:48:58,786 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/usr/lib/jvm/java-1.8-openjdk/jre
2018-08-11 09:48:58,786 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/zookeeper-3.4.13/bin/../build/classes:/zookeeper-3.4.13/bin/../build/lib/*.jar:/zookeeper-3.4.13/bin/../lib/slf4j-log4j12-1.7.25.jar:/zookeeper-3.4.13/bin/../lib/slf4j-api-1.7.25.jar:/zookeeper-3.4.13/bin/../lib/netty-3.10.6.Final.jar:/zookeeper-3.4.13/bin/../lib/log4j-1.2.17.jar:/zookeeper-3.4.13/bin/../lib/jline-0.9.94.jar:/zookeeper-3.4.13/bin/../lib/audience-annotations-0.5.0.jar:/zookeeper-3.4.13/bin/../zookeeper-3.4.13.jar:/zookeeper-3.4.13/bin/../src/java/lib/*.jar:/conf:
2018-08-11 09:48:58,787 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/usr/lib/jvm/java-1.8-openjdk/jre/lib/amd64/server:/usr/lib/jvm/java-1.8-openjdk/jre/lib/amd64:/usr/lib/jvm/java-1.8-openjdk/jre/../lib/amd64:/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2018-08-11 09:48:58,787 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2018-08-11 09:48:58,787 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2018-08-11 09:48:58,788 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2018-08-11 09:48:58,788 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2018-08-11 09:48:58,788 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=3.10.0-862.el7.x86_64
2018-08-11 09:48:58,788 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2018-08-11 09:48:58,788 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2018-08-11 09:48:58,789 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/zookeeper-3.4.13
2018-08-11 09:48:58,791 [myid:] - INFO  [main:ZooKeeper@442] - Initiating client connection, connectString=zk-1:2181 sessionTimeout=5000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@4b85612c
Welcome to ZooKeeper!
2018-08-11 09:48:58,842 [myid:] - INFO  [main-SendThread(zk-1:2181):ClientCnxn$SendThread@1029] - Opening socket connection to server zk-1/172.18.55.2:2181. Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2018-08-11 09:48:59,007 [myid:] - INFO  [main-SendThread(zk-1:2181):ClientCnxn$SendThread@879] - Socket connection established to zk-1/172.18.55.2:2181, initiating session
2018-08-11 09:48:59,048 [myid:] - INFO  [main-SendThread(zk-1:2181):ClientCnxn$SendThread@1303] - Session establishment complete on server zk-1/172.18.55.2:2181, sessionid = 0x100010c6f450005, negotiated timeout = 5000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zk: zk-1:2181(CONNECTED) 0] create /node_1 123
Node already exists: /node_1
[zk: zk-1:2181(CONNECTED) 1] get /node_1
123
cZxid = 0x100000002
ctime = Sat Aug 11 08:08:53 GMT 2018
mZxid = 0x100000002
mtime = Sat Aug 11 08:08:53 GMT 2018
pZxid = 0x10000001d
cversion = 2
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 3
numChildren = 0
[zk: zk-1:2181(CONNECTED) 2] quit

bash-4.4# zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Mode: follower
```

### Trouble shooting

- Edit /etc/hosts

```sh
bash-4.4# vi /etc/hosts

172.18.55.2     zk-1
172.18.101.3    zk-2
172.18.87.3     zk-3
```
