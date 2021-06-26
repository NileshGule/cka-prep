# Using Taints to control pod deployments

## Create nginx deployment with 8 replicas

```bash

kubectl apply -f taint-deployment.yaml

```

## taint worker node 2 with PreferNoSchedule effect

```bash

kubectl taint node kube-worker-2 bubba=value:PreferNoSchedule

```

## Remove taint worker node 2 with PreferNoSchedule effect

```bash

kubectl taint node kube-worker-2 bubba-

```

## taint worker node 2 with NoSchedule effect

```bash

kubectl taint node kube-worker-2 bubba=value:NoSchedule

```

## Untaint worker node 2 with NoSchedule effect

```bash

kubectl taint node kube-worker-2 bubba-

```

## taint worker node 2 with NoExecute effect

```bash

kubectl taint node kube-worker-2 bubba=value:NoExecute

```

## Untaint worker node 2 with NoExecute effect

```bash

kubectl taint node kube-worker-2 bubba-

```