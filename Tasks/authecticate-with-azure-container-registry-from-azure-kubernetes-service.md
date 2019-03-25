## Authenticate with Azure Container Registry from Azure Kubernetes Service

### Access with Kubernetes secret

Use the following kubectl command to create the Kubernetes secret. 

Replace <acr-login-server> with the fully qualified name of your Azure container registry (it's in the format "acrname.azurecr.io"). 

Replace <service-principal-ID> and <service-principal-password> with the values you obtained by running the previous script. 

Replace <email-address> with any well-formed email address.

```console
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Visit Home > Container registries > ${your acr name} > Access keys for the values of Login server, Username and password.

