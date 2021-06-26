# Helm

## Install Helm on master node

```bash

wget https://get.helm.sh/helm-v3.6.1-linux-amd64.tar.gz

tar -xvf helm-v3.6.1-linux-amd64.tar.gz

```

## Copy helm binary to `usr/local/bin` directory

```bash

sudo cp linux-amd64/helm /usr/local/bin/helm

```

## Search for Helm repos

```bash

helm search hub database

```

