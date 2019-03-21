### Install kubectl in your CLI

```console
sudo az aks install-cli
```

### Get the credentials for your cluster 

```console
sudo az aks get-credentials --resource-group uat-aks --name uat-aks
```

### Open the Kubernetes dashboard

```console
sudo az aks browse --resource-group uat-aks --name uat-aks
```
