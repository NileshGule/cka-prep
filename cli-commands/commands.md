# Azure CLI commands to mange master and worker nodes from terminal

## Start VMs

```bash

az vm start --resource-group demo-kubeadm --name kube-master

az vm start --resource-group demo-kubeadm --name kube-worker-1

az vm start --resource-group demo-kubeadm --name kube-worker-2

```

## Stop VMs

```bash

az vm stop --resource-group demo-kubeadm --name kube-master

az vm stop --resource-group demo-kubeadm --name kube-worker-1

az vm stop --resource-group demo-kubeadm --name kube-worker-2

```

## Restart VMs

```bash

az vm restart --resource-group demo-kubeadm --name kube-master

az vm restart --resource-group demo-kubeadm --name kube-worker-1

az vm restart --resource-group demo-kubeadm --name kube-worker-2

```