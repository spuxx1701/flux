# Flux

This repository houses the cluster configuration for my Kubernetes cluster.

## Bootstrap Flux
The repository is maintained with Flux. In case of a new fresh install of the cluster, Flux needs to be bootstrapped.

```bash
flux bootstrap github \
  --owner=spuxx1701 \
  --repository=flux \
  --branch=master \
  --token-auth \
  --path=./cluster \
  --components-extra=image-automation-controller,image-reflector-controller
```

When prompted, enter the PAT that Flux can use to access this repository.

Flux requires the GPG key for decrypting secrets. Put the key into a text file and add it to Kubernetes like so:
```bash
gpg --export-secret-keys --armor <key_id> | kubectl create secret generic sops-gpg --namespace=flux-system --from-file=sops.asc=/dev/stdin
```

## Setting up persistent storage

## Option 1: Hostpath storage

Microk8s offers an addon to easily implement persistent storage via a directory on the host sytem. The guide can be found [here](https://microk8s.io/docs/addon-hostpath-storage).

```bash
microk8s enable hostpath-storage
```
