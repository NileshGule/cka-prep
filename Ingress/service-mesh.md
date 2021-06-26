# Linkerd service mesh

## Set up linkerd

```bash

curl -sL run.linkerd.io/install | sh

```

## Add linked cli to the PATH

```bash

export PATH=$PATH:/home/azuser/.linkerd2/bin

export PATH=$PATH:$HOME/.linkerd2/bin

echo "export PATH=$PATH:$HOME/.linkerd2/bin" >> $HOME/.bashrc

```

## Run linkerd specific commands

```bash

linkerd check --pre                     # validate that Linkerd can be installed
  linkerd install | kubectl apply -f -    # install the control plane into the 'linkerd' namespace
  linkerd check                           # validate everything worked!
  linkerd viz install | kubectl apply -f -

  linkerd viz check

  linkerd viz dashboard &                       # launch the dashboard

  ```

## Edit the deployment to expose Linkerd dashboard outside the cluster

```bash

kubectl -n linkerd-viz edit deploy web

```
Edit setting for --enforced-hosts=

## Edit linkerd service to be of type NodePort

```bash

kubectl -n linkerdviz edit svc web

```

Add friendly nodePort: 31500 and change type of service from LoadBalancer to NodePort