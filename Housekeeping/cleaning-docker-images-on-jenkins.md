```console
#!/bin/bash -xe

df -h

if [[ $(docker ps -a -f status=exited -q) ]]; then 
    docker rm $(docker ps -a -f status=exited -q); 
else 
    echo "No exited containers!"; 
fi

# Delete dangling images
if [[ $(docker images -f "dangling=true" -q) ]]; then 
    docker rmi -f $(docker images -f "dangling=true" -q)
else 
    echo "No dangling images!"; 
fi

docker container prune --filter "until=12h" -f
# docker system prune --volumes -f

df -h
```

