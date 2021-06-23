# Basic node maintenance

## Backup etcd database

Location of etcd data directory demon

```bash

sudo grep data-dir /etc/kubernetes/manifests/etcd.yaml

```

- --data-dir=/var/lib/etcd

### exec into etcd container

```bash

kubectl get pods --namespace kube-system

kubectl --namespace kube-system exec -it etcd-<<name of the pod>> -- sh

```

### Navigate to the location of files where TLS related files are stored

```bash

cd /etc/kubernetes/pki/etcd

$ echo *
ca.crt ca.key healthcheck-client.crt healthcheck-client.key peer.crt peer.key server.crt server.key

```

Exit out of container.

### Check database health using loopback address and port 2379

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl endpoint health"

```

### Determine number of databases which are part of the cluster

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl --endpoints=https://127.0.0.1:2379 member list"

```

Command using relative paths to pki files (currently not working)

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
--cert=./peer.crt \
--key=./peer.key \
--cacert=./ca.crt \
etcdctl --endpoints=https://127.0.0.1:2379 member list"

```

Get results in tabular format

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl --endpoints=https://127.0.0.1:2379 -w table member list"

```

View the status of cluster in table format (not working)

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl --endpoints=https://127.0.0.1:2379 status"

```

backup etcd database

View the status of cluster in table format

```bash

kubectl -n kube-system exec -it etcd-kube-master -- sh \
-c "ETDCCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl --endpoints=https://127.0.0.1:2379 \
snapshot save /var/lib/etcd/snapshot.db"

```

Copy file from container to local (fails due to missing tar command inside the container)

```bash

kubectl cp kube-system/etcd-kube-master:/var/lib/etcd/snapshot.db  /home/azureuser/backup/snapshot.db

```

### backup snapshot and other files

```bash

mkdir $HOME/backup

sudo cp /var/lib/etcd/snapshot.db $HOME/backup/snapshot.db-$(date +%m-%d-%y)

sudo cp /root/kubeadm-config.yaml $HOME/backup/

sudo cp -r /etc/kubernetes/pki/etcd/ $HOME/backup/

```

---

## Upgrade cluster

### get list of available packages

```bash

sudo apt-cache madison kubeadm

```

Remove hold on kubeadm

```bash

sudo apt-mark unhold kubeadm

```

Update the package to 1.20.1

```bash

sudo apt-get install -y kubeadm=1.20.1-00

```

Already on 1.20.1
Upgrade to 1.20.2

```bash

sudo apt-get install -y kubeadm=1.20.2-00

```

### Drain master by ignoring the demonsets

```bash

kubectl drain kube-master --ignore-daemonsets

```

### Run upgrade plan

```bash

sudo kubeadm upgrade plan

```

### Upgrade the cluster

```bash

sudo kubeadm upgrade apply v1.20.2

```

### Release hold on kubelet and kubectl

```bash

sudo apt-mark unhold kubelet kubectl

```

### Upgrade kubelet and kubectl version to same as kubeadm

```bash

sudo apt-get install -y kubelet=1.20.2-00 kubectl=1.20.2-00

```

### Hold kubelet and kubectl so other updates don't upgrade

```bash

sudo apt-mark hold kubectl kubelet

```

### Restart the daemons

```bash

sudo systemctl daemon-reload

sudo systemctl restart kubelet

```

### Uncordon master nod

```bash

kubectl uncordon kube-master

```

---

## Upgrade worker nodes

### Unhold kubeadm

```bash

sudo apt-mark unhold kubeadm

```

### Update kubeadm to same version as on master

```bash

sudo apt-get install -y kubeadm=1.20.2-00

```

### hold kubeadm

```bash

sudo apt-mark hold kubeadm

```

### drain worker from the master node

```bash

kubectl drain kube-worker-1 --ignore-daemonsets

```

### Upgrade worker node by running command on worker

```bash

sudo kubeadm upgrade node

```

### Remove hold on kubelet and kubectl

```bash

sudo apt-mark unhold kubectl kubelet

```

### Update the version for kubelet and kubectl to match the one on the master

```bash

sudo apt-get install kubectl=1.20.2-00 kubelet=1.20.2-00

```

### Put the packages on hold

```bash

sudo apt-mark hold kubectl kubelet

```

### Restart daemon process

```bash

sudo systemctl daemon-reload

sudo systemctl restart kubelet

```

### Uncordon the node

```bash

kubectl uncordon kube-worker-1

```
