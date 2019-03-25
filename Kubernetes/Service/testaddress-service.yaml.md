```yaml
apiVersion: v1
kind: Service
metadata:
  name: testaddress-service-v0-1
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  selector:
    app: testaddress-service-v0-1
```