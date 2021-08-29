# TLS related commands

## View kubectl on master and worker nodes

```bash

systemctl status kubectl.service

```

## Check settings in /var/lib/kubelet/config.yaml

```bash

sudo less /var/lib/kubelet/config.yaml

```