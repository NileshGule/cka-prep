# Nginx ingress

## Search for Nginx ingress Helm chart

```bash

helm search hub ingress

```

## Add Nginx helm repo

```bash

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update

```

## Edit values.yaml file to change `deployment` to `Daemonset`

```bash

helm fetch ingress-ngnix/ingress-nginx --untar

cd ingress-nginx

vi values.yaml

```

Change Deployment to DaemonSet

## install ingress

```bash

helm install myingress .

```

## verify ingress resources

```bash
kubectl get ingress --all-namespaces

kubectl get svc -o wide -w myingress-ingress-nginx-controller

```

## Create 2 nginx deployments and expose using service of type LoadBalancer

```bash

kubectl apply -f nginx-one-ingress.yaml

kubectl apply -f nginx-two-ingress.yaml

kubectl expose --name nginx-one-service deploy nginx-one-ingress

kubectl expose --name nginx-two-service deploy nginx-two-ingress

```

## Create ingress with nginx-one-service as backend

```bash

kubectl apply -f ingress.yaml

```

## Get service IP

```bash

kubectl get svc | grep nginx

```
Note the LodBalancer IP
10.101.30.12 

## verify outside access using ingress

```bash

curl 10.101.30.12

curl -H "Host: www.external.com" http:/10.101.30.12

```