# HAProxy setup

## Deploy HAProxy

```bash

sudo apt-get update; sudo apt-get install -y haproxy

```

## Edit config to add front end and backend servers

```bash

sudo vi /etc/haproxy/haproxy.cfg

```

## Restart haproxy service

```

sudo systemctl restart haproxy.service

sudo systemctl status haproxy.service

```

## Edit /etc/host on control plane node

```bash

sudo vi /etc/hosts

```

## Open port 9999 (if not already opened) on proxy node

```bash

az vm open-port `
--resource-group demo-kubeadm `
--name kubeadm-ha-proxy --port 9999

az vm open-port `
--resource-group demo-kubeadm `
--name kubeadm-ha-proxy --port 6443

```
