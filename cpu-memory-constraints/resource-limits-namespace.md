# Resource limits for namespace

## Create namespace

```bash

kubectl create namespace low-usage-limit

```

## Create LimitRange & assign to namespace

```bash

kubectl --namespace low-usage-limit create -f low-resource-limit-range.yaml

```

## Describe Limit Range

```bash

kubectl describe limitrange --namespace low-usage-limit

```

## Create deployment 

```bash

kubectl create deployment limited-hog --image vish/stress -n low-usage-limit

```