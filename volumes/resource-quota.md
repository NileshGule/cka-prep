# Resource quota to limit PVC count and usage

## Create 300M file inside /opt/sfw on host

```bash

sudo dd if=/dev/zero of=/opt/sfw/bigfile bs=1M count=300

```

## recreate PV

```bash

kubectl create -f persistent-volume.yaml

```

## Patch the Retention Policy to `Delete`

```bash

kubectl patch pv pvvol-1 -p \
'{"spec:":{"persistentVolumeReclaimPolicy":"Delete"}}'

```

## Remove existing resource quota from namespace

```bash

kubectl -n small delete resourcequota storage-quota

```

## Limit storage quota to 100M

Edit storage-quota.yaml
