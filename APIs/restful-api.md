# Explore RESTful API

## Get server details from config

```bash

kubectl config view

```

Note down the server & port
172.0.0.4:6443

## Get secrets across al namespaces

```bash

kubectl get secret --all-namespaces

```

## Get teh secret from `default` namespace

```bash

kubectl get secret

```

## Retrieve token info from secret

```bash

kubectl describe secret default-token-<<token-random-name>>

```

## Extract token into an environment variable

```bash

export token=$(kubectl describe secret default-token-<<token-random-name>> | grep ^token | cut -f7 -d ' ')

```

## Get basic info from cluster

```bash

curl https://172.0.0.4:6443/apis --header "Authorization: Bearer $token" -k

```

## Get V1 version of APIs

```bash

curl https://172.0.0.4:6443/api/v1 --header "Authorization: Bearer $token" -k

```

## Set proxy

```bash

kubectl proxy --api-prefix=/

curl http://127.0.0.1:8001/api/

```