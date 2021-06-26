# Nodeport service

## Expose service as nodeport type

```bash

kubectl expose deployment nginx-one --type=NodePort --name=nginx-nodeport