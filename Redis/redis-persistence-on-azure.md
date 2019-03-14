## Create storage account on Azure

```console
faming_hou@Azure:~/storage/redis_persistence$ az aks show --resource-group aks --name ${name}-aks --query nodeResourceGroup -o tsv
MC_aks_${name}-aks_westus

faming_hou@Azure:~/storage/redis_persistence$ az storage account create --resource-group MC_aks_${name}-aks_westus --name ${name}storageaccount --sku Standard_LRS

faming_hou@Azure:~/storage/redis_persistence$ az storage account create --resource-group MC_aks_${name}-aks_westus --name ${name}storageaccount --sku Standard_LRS
{
  "accessTier": null,
  "creationTime": "2018-10-16T00:59:52.515725+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": {
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "services": {
      "blob": {
        "enabled": true,
        "lastEnabledTime": "2018-10-16T00:59:52.656494+00:00"
      },
      "file": {
        "enabled": true,
        "lastEnabledTime": "2018-10-16T00:59:52.656494+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "id": "/subscriptions/5a910394-43ce-4392-9923-fcf9e4cfcb36/resourceGroups/MC_aks_${name}-aks_westus/providers/Microsoft.Storage/storageAccounts/${name}storageaccount",
  "identity": null,
  "isHnsEnabled": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "westus",
  "name": "${name}storageaccount",
  "networkRuleSet": {
    "bypass": "AzureServices",
    "defaultAction": "Allow",
    "ipRules": [],
    "virtualNetworkRules": []
  },
  "primaryEndpoints": {
    "blob": "https://${name}storageaccount.blob.core.windows.net/",
    "dfs": null,
    "file": "https://${name}storageaccount.file.core.windows.net/",
    "queue": "https://${name}storageaccount.queue.core.windows.net/",
    "table": "https://${name}storageaccount.table.core.windows.net/",
    "web": null
  },
  "primaryLocation": "westus",
  "provisioningState": "Succeeded",
  "resourceGroup": "MC_aks_${name}-aks_westus",
  "secondaryEndpoints": null,
  "secondaryLocation": null,
  "sku": {
    "capabilities": null,
    "kind": null,
    "locations": null,
    "name": "Standard_LRS",
    "resourceType": null,
    "restrictions": null,
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": null,
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
faming_hou@Azure:~/storage/redis_persistence$

```

## Create StorageClass

```console
faming_hou@Azure:~/storage/redis$ more azure-file-sc.yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
  storageAccount: ${name}storageaccount
faming_hou@Azure:~/storage/redis$
```

```console
kubectl apply -f azure-file-sc.yaml
```

## Create ClusterRole

```console
faming_hou@Azure:~/storage/redis$ more azure-pvc-roles.yaml
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system

faming_hou@Azure:~/storage/redis$
```

```console
kubectl apply -f azure-pvc-roles.yaml

```

## Create PersistentVolumeClaim

```console
faming_hou@Azure:~/storage/redis$ more azure-file-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi

faming_hou@Azure:~/storage/redis$
```

```console
kubectl apply -f azure-file-pvc.yaml

```

## Deploy StatefulSet

```console
faming_hou@Azure:~/statefulset$ more redis_statefulset.yaml
apiVersion: apps/v1beta2
kind: StatefulSet
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis  # has to match .spec.template.metadata.labels
  serviceName: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis  # has to match .spec.selector.matchLabels
    spec:
      containers:
        - name: redis
          image: redis:3.2-alpine
          imagePullPolicy: Always
          ports:
            - containerPort: 6379
              name: redis
          volumeMounts:
            - name: redis-volume
              mountPath: /data
      volumes:
        - name: redis-volume
          persistentVolumeClaim:
            claimName: azurefile


faming_hou@Azure:~/statefulset$
```

## Deploy Service

```console
faming_hou@Azure:~/service$ more redis_service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  ports:
  - port: 6379
    name: redis
  type: LoadBalancer
  selector:
    app: redis

faming_hou@Azure:~/service$
```

```console
kubectl apply -f  redis_service.yaml
```

## Test Redis service on Azure

```console
faming_hou@Azure:~/redis_persistence_test$ kubectl exec redis-0 date
Mon Oct 15 20:32:16 UTC 2018
faming_hou@Azure:~/redis_persistence_test$ kubectl exec redis-0 -it -- redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> get k1
(nil)
127.0.0.1:6379> get k2
(nil)
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> get k1
"v1"
127.0.0.1:6379>exit
```

## List all keys in pod redis-0 (Do not do it in production environment)

```console
kubectl exec redis-0 -it -- redis-cli keys *
```

## References

* [Redis Persistence](https://redis.io/topics/persistence)
* [Setup Persistence Redis cluster in Kubernetes](https://medium.com/zero-to/setup-persistence-redis-cluster-in-kubertenes-7d5b7ffdbd98)

