# Assign Pod using Labels

## view current labels on nodes

```bash

kubectl describe nodes | grep -A5 -i label

```

## Get count of containers running on each node using both docker as well as cri-o

```bash

sudo docker ps | wc -l

sudo crictl ps | wc -l

```

## Add VIP label to master and other to worker nodes

```bash

kubectl label nodes kube-master status=vip

kubectl label nodes kube-worker-1 status=other

kubectl label nodes kube-worker-2 status=other

```