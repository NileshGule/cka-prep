# Manage resources using labels

## delete pods matching label

```bash

kubectl delete pod -l app=secondary --all-namespaces

kubectl delete pod -l app=secondary -A

```

## Get all resources across all namespaces having a specific label `app=secondary`

```bash

kubectl get all -l app=secondary -A

kubectl get all -l app=secondary --all-namespaces

```