## Open a shell on a pod

```console
kubectl exec -it commonnginx-service-v0-1-6858b66b8c-z8rp8 -- /bin/bash
```

## Copy nginx.conf on Jenkins onto the pod of K8s

```console
kubectl cp /var/lib/jenkins/jobs/${JOB_NAME}/workspace/src/CommonnginxService/nginx.conf default/$commonnginx_pod:/etc/nginx/;
```

## Copy a new nginx.conf onto a pod and then reload nginx  ('commonnginx' is the service name)

```console
#!/bin/bash -xe

echo ${JOB_NAME}

commonnginx_pod=$(kubectl get pods |grep commonnginx | awk -F ' ' '{ print $1 }')

if [[ $commonnginx_pod ]]; then 
    kubectl cp /var/lib/jenkins/jobs/${JOB_NAME}/workspace/src/CommonnginxService/nginx.conf default/$commonnginx_pod:/etc/nginx/; 
    kubectl exec $commonnginx_pod -it -- /etc/init.d/nginx configtest
    kubectl exec $commonnginx_pod -it -- /etc/init.d/nginx reload
else 
    echo "No commonnginx pod!"; 
fi
```