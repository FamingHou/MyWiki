## The Dockerfile of nginx service

```console
FROM nginx:1.15.8

WORKDIR "/src/CommonnginxService"

RUN apt-get update && apt-get install -y curl \
    && mkdir -p /opt/bin 

COPY startup.sh /opt/bin

RUN pwd && ls -al

RUN cd /opt/bin \
    && chmod +x startup.sh \
    && pwd && ls -al

EXPOSE 80

ENTRYPOINT ["/opt/bin/startup.sh"]
```

## startup.sh

```console
#!/bin/bash

cd /etc/nginx

curl -O https://${hostname}.aliyuncs.com/nginx.conf

cat nginx.conf

# /etc/init.d/nginx -g 'daemon off;'
/etc/init.d/nginx start -g 'daemon off;'

tail -f /var/log/nginx/error.log
```

