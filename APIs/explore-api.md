# Explore API calls  

## install `strace` utility

```bash

sudo apt-get install strace -y

```

## Explore endpoints using strace

```

strace kubectl get endpoints

```

## Navigate to cache

```bash

cd ~/.kube/cache/discovery/<<ipOrHostname_6443>>

```

## View objects available in v1 of API

```bash

python3 -m json.tool v1/serverresources.json

```
