### Check Docker version

```
PS C:\Users\frank> docker --version
Docker version 18.09.2, build 6247962
```

### Download a tagged image 

```
PS C:\Users\frank> docker pull redis:5.0.7
5.0.7: Pulling from library/redis
Digest: sha256:0e3d2d3cefbcef73fd10d0a6ccf5aea320b2037dd7b995830fd73888fa33ae58
Status: Downloaded newer image for redis:5.0.7
```

### Run Redis process in an isolated container [(reference)](https://docs.docker.com/engine/reference/run/)

```
PS C:\Users\frank> docker run -p 6379:6379 --name myredis -d redis
ca07e69d26f39babee03157228ade279c5764d8192470d20342608046b18b02c
```

### List Docker containers [(reference)](https://docs.docker.com/engine/reference/commandline/ps/)

```
PS C:\Users\frank> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
ca07e69d26f3        redis               "docker-entrypoint.s…"   7 seconds ago       Up 5 seconds        0.0.0.0:6379->6379/tcp   myredis
```

### View the log

```
PS C:\Users\frank> docker logs myredis
```

### Start a shell inside the container

```
PS C:\Users\frank> docker exec -it myredis sh
#
```

### Run redis-cli in the container

```
### redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

