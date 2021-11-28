# Network policy related commands

## Create nginx deployment and expose via service

```

kubectl create deploy nginx --image=nginx

kubectl expose deploy nginx --port=80

```

## Test service by accessing from another pod

``` 

kubectl run --rm -it busybox --image=busybox -- /bin/sh

wget --spider --timeout=1 nginx

```

## Limit access to nginx service

Create network policy to limit service access from pods with label `access:true`

```yaml

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-nginx
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: "true"

```

## Access from pod without label

```

kubectl run --rm -it busybox --image=busybox -- /bin/sh 

wget --spider --timeout=1 nginx

```

## Access from pod with label

```

kubectl run --rm -it --labels="access=true" busybox --image=busybox -- /bin/sh 

wget --spider --timeout=1 nginx

```