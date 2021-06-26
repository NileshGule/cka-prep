# Working with CoreDNS

## Create a pod with network tools

```bash

kubectl create -f nettool.yaml

kubectl exec -it -- sh

```

## Install curl & dnutils inside the container

```bash

apt-get update && apt-get install curl dnsutils -y

```

## Run network related commands

```bash

dig

cat /etc/resolv.conf

dig @10.96.0.10 -x 10.96.0.10

curl nginx-nodeport.default.svc.cluster.local

curl nginx-nodeport

curl nginx-nodeport.default

```

## Check details of config volumes for kube-system namespaced pods

```bash

kubectl -n kube-system get po coredns-85d9df8444-pdxjn -o yaml

```

## View configmaps in kube-system namespace

```bash

kubectl -n kube-system get cm

kubectl -n kube-system describe cm coredns

```