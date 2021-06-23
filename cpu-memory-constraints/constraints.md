# Constraints

## Deploy container using deployment

```bash

kubectl create deploy hog --image vish/stress

```

## Customize the yaml to add memory limits

```bash

kubectl get deployment hog --output yaml > hog.yaml

```