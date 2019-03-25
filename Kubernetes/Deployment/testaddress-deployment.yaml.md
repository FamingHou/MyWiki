```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: testaddress-service-v0-1
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: testaddress-service-v0-1
    spec:
      containers:
      - name: testaddress-service-v0-1
        image: ${projectname}.azurecr.io/testaddressservice:v0.1.34
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
          requests:
            memory: "50Mi"
            cpu: "50m"
        ports:
        - containerPort: 80
      imagePullSecrets:
      - name: ${projectname}-secret-name
```