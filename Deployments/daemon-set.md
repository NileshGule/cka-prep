# DaemonSet

## View current update strategy for DaemonSet

```bash

kubectl get ds ds-one -o yaml | grep -A 4 Strategy

```

## Edit strategy to `OnDelete` to allow manual termination of some pods

```bash 

kubectl edit ds ds-one

```

## Update nginx image to `1.16.1-alpine`

```bash

kubectl set image ds ds-one nginx=nginx:1.16.1-alpine

```

## view rollout history

```bash

kubectl rollout history ds ds-one

```

## view rollout history for specific revision

```bash

kubectl rollout history ds ds-one --revision=1

```

## Rollback to version 1

```bash

kubectl rollout undo ds ds-one --to-revision=1

```