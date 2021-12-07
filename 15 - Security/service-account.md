# Kubernetes RBAC service accounts

Refer to the [Youtube video](https://youtu.be/keoYFZhtg0U)

## Run alpine pod

```

kubectl run -it --rm alpine --image=alpine -- sh

cd /var/run/secrets/kubernetes.io/serviceaccount/

```

## Create service account

```

kubectl create serviceaccount appsa --dry-run=client -o yaml

kubectl create serviceaccount appsa

```

## decode the base64 encoded token from the secret associated with service account

```

kubectl get secret appsa-token-<<random-value>> -o jsonpath='{.data.token}' | base64 -d


```

## Create a pod which uses service account 

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: alpine
  name: alpine-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: alpine
  strategy: {}
  template:
    metadata:
      labels:
        app: alpine
    spec:
      serviceAccountName: appsa
      containers:
      - image: byrnedo/alpine-curl
        name: alpine-curl
        resources: {}
        command:
          - "sh"
          - "-c"
          - "sleep 10000"

```

## Exec into the pod

```

 kubectl exec -it alpine-deployment-<<random value>> -- sh

 ```

## create variables to store the service account secret nad token

```

CA=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
TOKEN=$(cat /var/run/sercrets/kubernetes.io/serviceaccount/token)

```

## Use curl to query the kubernetes API from inside the pod

```

curl --cacert $CA -X GET https://kubernetes/api --header "Authorization: Bearer $TOKEN"

```

## Create Role `podlister` 

```

kubectl create role podlister --verb=list --resource=pods --dry-run=client -o yaml

kubectl create role podlister --verb=list --resource=pods

```

## Create Rolebinding

```

kubectl create rolebinding appsa-podlist-role-binding  --role=podlister --serviceaccount=default:appsa --dry-run=client -o yaml

kubectl create rolebinding appsa-podlist-role-binding  --role=podlister --serviceaccount=default:appsa

```

## Limit the number of lines in the output

```

curl --cacert $CA -X GET https://kubernetes/api/v1/namespaces/default/pods --header "Authorization: Bearer $TOKEN" | head -n 20

```

