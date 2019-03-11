## Download the docker image from ACR running on AKS

```console
kubectl create secret docker-registry ${yourcompany}-secret-name --docker-server=${yourcompany}.azurecr.io --docker-username=${yourcompany} --docker-password=${yourpassword} --docker-email=${youremail}
```
[Authenticate with Azure Container Registry from Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks#grant-aks-access-to-acr)
