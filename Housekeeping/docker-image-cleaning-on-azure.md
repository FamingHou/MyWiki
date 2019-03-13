##  Requirements

Delete docker images and keep only the latest two versions on Azure.

## Solution

clean-azure-images.sh

```
#!/bin/bash

echo "Deleting images..."

az acr repository list -n microshop | awk -F '["]' '{ print $2 }' | while read x; do
    if [[ $x == *service ]]; then    
        i=0
        echo "Processing $x"
        az acr repository show-tags -n microshop --repository $x --orderby time_desc | awk -F '["]' '{ print $2 }' | while read y; do
            if [[ $y == v* ]]; then
                i=$(( $i + 1))
                echo "index $i"
                if [[ $i -ge 3 ]]; then
                    echo "Deleting $x:$y"
                    az acr repository delete -n microshop --image $x:$y --yes
                fi
            fi            
        done
    fi    
done

echo "Done. There should be no more than 2 latest versions of each service."
```

### Shells

1. Login Azure
2. Execute the shell
```cons
faming_hou@Azure:~$ cd /home/faming_hou/housekeeping/bin
faming_hou@Azure:~/housekeeping/bin$ ./clean-azure-images.sh
```

