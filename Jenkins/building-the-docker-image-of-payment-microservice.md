```console
#!/bin/bash -xe

# Variables
NAME=payment
MAJOR=0
MINOR=1

# Please do not modify the followings
USERNAME=${yourusername}
PASSWORD=${yourpassword}
IMAGE=${NAME}service:v${MAJOR}.${MINOR}.${BUILD_NUMBER}
cd ./src
pwd
cp ./${NAME^}Service/Dockerfile .
# Build service by using Dockerfile with tag latest
docker build . --tag  ${company}.azurecr.io/${IMAGE} --file Dockerfile --rm=false
# Docker login to  ${company}.azurecr.io with username & password
docker login  ${company}.azurecr.io -u $USERNAME -p $PASSWORD
# Push image
docker push  ${company}.azurecr.io/${IMAGE}
# Connect to the cluster on AKS 
# az aks get-credentials --resource-group aks --name  ${company}-aks
# Deploy this version onto Kubernetes of Azure
kubectl set image deployment/${NAME}-service-v${MAJOR}-${MINOR} ${NAME}-service-v${MAJOR}-${MINOR}= ${company}.azurecr.io/${IMAGE}
# Remove images
docker rmi  ${company}.azurecr.io/${IMAGE}
```

