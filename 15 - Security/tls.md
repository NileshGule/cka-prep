# TLS related commands

## View kubectl on master and worker nodes

```bash

systemctl status kubectl.service

```

## Check settings in /var/lib/kubelet/config.yaml

```bash

sudo less /var/lib/kubelet/config.yaml

sudo less /etc/kubernetes/manifests/kube-controller-manager.yaml

```

## List secrets in kube-system namespace

```bash

kubectl get secrets --namespace kube-system

kubectl -n kube-system get secrets certificate<<>> -o yaml

```

## Copy access configuration file

```bash

cp $HOME/.kube/config $HOME/cluster-api-config

```

kubectl config <tab><tab>

## Work with kubeadm

```bash

sudo kubeadm token -h

sudo kubeadm config -h

sudo kubeadm config print init-defaults

```