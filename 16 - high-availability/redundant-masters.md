# Additional masters setup

## Configure & install Kubernetes on additional master nodes

```bash

sudo -i

apt-get update && apt-get upgrade -y

```

## Install container engine

```bash

apt-get install -y docker.io

```

## Configure software repo and add Kubernetes

```bash

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" \
>> /etc/apt/sources.list.d/kubernetes.list

curl -s \
https://packages.cloud.google.com/apt/doc/apt-key.gpg \
| apt-key add -

apt-get update

apt-get install -y \
kubeadm=1.22.3-00 kubelet=1.22.3-00 kubectl=1.22.3-00

apt-mark hold kubelet kubeadm kubectl

```

## Create hashes and tokens on the first master node necessary to join the cluster

```bash

sudo kubeadm token create

```

## Create new SSL hash

```bash

openssl x509 -pubkey \
-in /etc/kubernetes/pki/ca.crt | openssl rsa \
-pubin -outform der 2>/dev/null | openssl dgst \
-sha256 -hex | sed 's/^.* //'


```

## Create new certificate to join as control plane instead of worker

```bash

sudo kubeadm init phase upload-certs --upload-certs


```

## Join as control plane node instead of worker using kubeadm

```bash

sudo kubeadm join k8scp:6443 \
--token <<>> \
--discovery-token-ca-cert-hash <<>> \
--control-plane --certificate-key \
<<>>

```

## Copy the configuration file 

```bash

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

```