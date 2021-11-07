# Authentication and Authorization

## Create Production and developement namespaces

```bash

kubectl create namespace development

kubectl create namespace production

```

## Get contexts

```bash

kubectl config get-contexts

```

## Create user and assign password

```bash

sudo useradd -s /bin/bash DevDan

sudo passrd DevDan

```

## Generate private key Certificate Signing request (CSR) for DevDan

```bash

openssl genrsa -out DevDan.key 2048

touch $HOME/.rnd

openssl req -new -key DevDan.key \
-out DevDan.csr -subj "/CN=DevDan/O=development"

```

## Generate self-signed certificate using x509 protocol

```bash

sudo openssl x509 -req -in DevDan.csr \
-CA /etc/kubernetes/pki/ca.crt \
-CAkey /etc/kubernetes/pki/ca.key \
-CAcreateserial \
-out DevDan.crt -days 45

```

## Update access config file to reference the new key and certificate

```bash

kubectl config set-credentials DevDan \
--client-certificate=$HOME/DevDan.crt \
--client-key=$HOME/DevDan.key

```

## Create context

```bash

kubectl config set-context DevDan-context \
--cluster=kubernetes \
--namespace=development \
--user=DevDan

```

```bash

kubectl config set-context ProdDan-context \
--cluster=kubernetes \
--namespace=production \
--user=DevDan

