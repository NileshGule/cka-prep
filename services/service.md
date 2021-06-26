# Services

## Create deployment with 2 replicas using [nginx-one.yaml](nginx-one.yaml)

```bash

kubectl create -f nginx-one.yaml

```

## Label worker-node-2 

```bash

kubectl label node kube-worker-2 system=secondOne

```

## Filter pods based on label

```bash

kubectl get pods -l app=secondary

kubectl get pods --selector app=secondary

```

## Expose the deployment

```bash

kubectl expose deployment nginx-one

```

kubectl get svc