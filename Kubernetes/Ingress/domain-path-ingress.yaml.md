```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: api-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/configuration-snippet: |
      proxy_ignore_client_abort on;
    name: rewrite
    namespace: default
spec:
  #tls:
  #- hosts:
  #  - k8s-dashboard.api.com
  #  secretName: kubernetes-dashboard-certs
  rules:
  - host: dashboard.${projectname}.com
    http:
      paths:
      - path: '/'
        backend:
          serviceName: dashboard-service-v0-1
          servicePort: 80
  - host: dashboard.${projectname}.co.nz
    http:
      paths:
      - path: '/'
        backend:
          serviceName: dashboard-service-v0-1
          servicePort: 80
  - host: dashboard.${projectname}.co.uk
    http:
      paths:
      - path: '/'
        backend:
          serviceName: dashboard-service-v0-1
          servicePort: 80
  - host: app.${projectname}.co.nz
    http:
      paths:
      - path: '/'
        backend:
          serviceName: app-service-v0-1
          servicePort: 80     
  - host: share.${projectname}.com
    http:
      paths:
      - path: '/'
        backend:
          serviceName: website-service-v0-1
          servicePort: 80
  - host: share.${projectname}.co.nz
    http:
      paths:
      - path: '/'
        backend:
          serviceName: website-service-v0-1
          servicePort: 80          
  - host: api.${projectname}.com
    http:
      paths:
      - path: '/address/v0.1/'
        backend:
          serviceName: address-service-v0-1
          servicePort: 80
      - path: '/admin/v0.1/'
        backend:
          serviceName: admin-service-v0-1
          servicePort: 80
      - path: '/captcha/v0.1/'
        backend:
          serviceName: captcha-service-v0-1
          servicePort: 80
      - path: '/cart/v0.1/'
        backend:
          serviceName: cart-service-v0-1
          servicePort: 80
      - path: '/client/v0.1/'
        backend:
          serviceName: client-service-v0-1
          servicePort: 80          
      - path: '/chat/v0.1/'
        backend:
          serviceName: chat-service-v0-1
          servicePort: 80          
      - path: '/courier/v0.1/'
        backend:
          serviceName: courier-service-v0-1
          servicePort: 80          
      - path: '/goods/v0.1/'
        backend:
          serviceName: goods-service-v0-1
          servicePort: 80
      - path: '/testgoods/v0.1/'
        backend:
          serviceName: testgoods-service-v0-1
          servicePort: 80
      - path: '/member/v0.1/'
        backend:
          serviceName: member-service-v0-1
          servicePort: 80
      - path: '/order/v0.1/'
        backend:
          serviceName: order-service-v0-1
          servicePort: 80
      - path: '/payment/v0.1/'
        backend:
          serviceName: payment-service-v0-1
          servicePort: 80
      - path: '/template/v0.1/'
        backend:
          serviceName: template-service-v0-1
          servicePort: 80
  - host: chat.${projectname}.com
    http:
      paths:
      - path: '/chat/v0.1/'
        backend:
          serviceName: chat-service-v0-1
          servicePort: 80          
```