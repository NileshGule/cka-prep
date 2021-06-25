# Persistent Volume

## Deploy NFS server on master node

```bash

sudo apt-get update && apt-get install -y nfs-kernel-server

```

## Create a directory with similar permissions as /tmp

```bash

sudo mkdir /opt/sfw

sudo chmod 1777 /opt/sfw

sudo bash -c 'echo software > /opt/sfw/hellp.txt'

```

## Edit NFS server file to share directory with all

```bash

sudo vi /etc/exports

/opt/sfw *(rw,sync,no_root_squash,subtree_check)

```

## re-read /etc/exports

```

sudo exportfs -ra

```

## Test by mounting resource from worker node

```

sudo apt-get install -y nfs-common

showmount -e kube-master

sudo mount kube-master:/opt/sfw /mnt

ls -l mnt

```