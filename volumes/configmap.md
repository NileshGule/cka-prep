# ConfigMap

## create configMap

```bash

kubectl create configmap colors \
--from-literal=text=black \
--from-file=./favorite \
--from-file=./primary/

```

## verify env variable is created

```bash

kubectl exec -it shell-demo -- /bin/sh -c  'echo $ilike'

```