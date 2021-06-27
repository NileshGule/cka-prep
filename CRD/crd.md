# Custom Resource Definition

## View existing CRDs

```bash

kubectl get crd --all-namespaces

kubectl get crd -A

```

## Create CRD

```bash

kubectl apply -f crd.yaml

```

## Create new object using CRD

```bash

kubectl apply -f crontab-crd.yaml

```

## verify

```bash

kubectl get CronTab

kubectl get ct

kubectl describe ct

```

## Remove CRD to cleanup resources

```bash

kubectl delete -f crd.yaml

```