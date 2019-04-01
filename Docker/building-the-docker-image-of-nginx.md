```console
#!/bin/bash -xe

# Variables
NAME=commonnginx
MAJOR=0
MINOR=1

# Please do not modify the followings 
USERNAME=${yourusername}
PASSWORD=${yourpassword}
IMAGE=${NAME}service:v${MAJOR}.${MINOR}.${BUILD_NUMBER}
cd ./src
pwd
cd ./${NAME^}Service
# Build service by using Dockerfile with tag latest
docker build . --tag ${yourname}.azurecr.io/${IMAGE} --file Dockerfile --rm=false
# Docker login to ${yourname}.azurecr.io with username & password
docker login ${yourname}.azurecr.io -u $USERNAME -p $PASSWORD
# Push image
docker push ${yourname}.azurecr.io/${IMAGE}
# Connect to the cluster on AKS 
# az aks get-credentials --resource-group aks --name ${yourname}-aks
# Deploy this version onto Kubernetes of Azure
kubectl set image deployment/${NAME}-service-v${MAJOR}-${MINOR} ${NAME}-service-v${MAJOR}-${MINOR}=${yourname}.azurecr.io/${IMAGE}
# Remove images
docker rmi ${yourname}.azurecr.io/${IMAGE}
```
