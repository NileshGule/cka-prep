# Working with user roles

Refer to the [Youtube video](https://youtu.be/U67OwM-e9rQ) for more details

## Create namespace `finance`

```

kubectl create ns finance

```

## Create private key for user named `john`

```

openssl genrsa -out john.ket 2048

```

## Create CSR request for user `john` with group as `finance`

```

openssl req -new -key john.key -out john.csr -subj "CN=john/O=finance"

```

## Sign using CA certificate & key

master node contains ca.crt and ca.key in hte /etc/kubernetes/pki directory

```

openssl x509 -req -in john.csr \
-CA /etc/kubernetes/pki/ca.crt \
-CAKey /etc/kubernetes/pki/ca.key \
-CAcreateserial -out john.crt -days 365

```

## Create kubeconfig file so user `john` can access Kubernetes cluster

Get the server url

```

kubectl config view

```

create kubeconfig file

```

kubectl --kubeconfig john.kubeconfig config set-cluster kubernetes --server https://172.0.0.4:6443 --certificate-authority=/etc/kubernetes/pki/ca.crt

```

Add user to kubeconfig

```

kubectl --kubeconfig john.kubeconfig config \
set-credentials john \
--client-certificate=/home/azuser/john.crt \
--client-key=/home/azuser/john.key

```

Add context

```

kubectl --kubeconfig john.kubeconfig config \
set-context john-context \
--cluster=kubernetes \
--user=john \
--namespace=finance

```

## Alternate option for adding kubeconfig file

Copy existing kubeconfig 

```

cp ~/.kube/config ~/.kube/john.config

```

Update user, name attributes for Context
Update current context
Update client-certificate-data and client-key-data attributes

base64 encode john.crt and john.key

```

cat john.crt | base64 -w0

cat john.key | base64 -w0

```
