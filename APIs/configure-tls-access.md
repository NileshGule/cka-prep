# Configure TLS access

## Set the client certificate data key

```bash

export client=$(grep client-cert $HOME/.kube/config | cut -d " " -f 6)

echo $client

```

## Set client-key-data

```bash

export key=$(grep client-key-data $HOME/.kube/config | cut -d " " -f 6)

echo $key

```

## Set certificate-authority-data

```bash

export auth=$(grep certificate-authority-data $HOME/.kube/config | cut -d " " -f 6)

echo $auth

```

## Encode keys to use with curl

```bash

echo $client | base64 -d > ./client.pem

echo $key | base64 -d > ./client-key.pem

echo $auth |  base64 -d > ./ca.pem

```

## Pull API server URL from config

```bash

kubectl config view | grep server

* - https://172.0.0.4:6443

```

## Hit API using `curl` command

```bash

curl --cert ./client.pem \
--key ./client-key.pem \
--cacert ./ca.pem \
https://172.0.0.4:6443/api/v1/pods

```