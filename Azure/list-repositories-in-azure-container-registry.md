## List repositories in an Azure Container Registry

```console
faming_hou@Azure:~$ az acr repository list -n ${MyRegistry}
[
  "addressservice",
  "adminservice",
  "appservice",
  "captchaservice",
  "cartservice",
  "chatservice",
  "clientservice",
  "dashboardservice",
  "goodsservice",
  "memberservice",
  "orderservice",
  "paymentservice",
  "templateservice",
  "websiteservice"
]
faming_hou@Azure:~$
```

## Show tags for a repository in an Azure Container Registry

```console
faming_hou@Azure:~$ az acr repository show-tags -n ${MyRegistry} --repository paymentservice --orderby time_desc
[
  "v0.1.77",
  "v0.1.75",
  "v0.1.74",
  "v0.1.73",
  "v0.1.72",
  "v0.1.71",
  "v0.1.70",
  "v0.1.69",
  "v0.1.68",
  "v0.1.67",
  "v0.1.66",
  "v0.1.65",
  "v0.1.64",
  "v0.1.63",
  "v0.1.62",
  "v0.1.61",
  "v0.1.60",
  "v0.1.59",
  "v0.1.58",
  "v0.1.57",
  "v0.1.56",
  "v0.1.55",
  "v1.0.0.53",
  "v1.0.0.52",
  "v1.0.0.51"
]
faming_hou@Azure:~$
```

## Show and filter tags for a repository in an Azure Container Registry

```console
faming_hou@Azure:~$ az acr repository show-tags -n ${MyRegistry} --repository addressservice --orderby=time_desc | awk -F '["]' '{ print $2 }'

v0.1.77
v0.1.75
v0.1.74
v0.1.73
v0.1.72
v0.1.71
v0.1.70
v0.1.69
v0.1.68
v0.1.67
v0.1.66
v0.1.65
v0.1.64
v0.1.63
v0.1.62
v0.1.61
v0.1.60
v0.1.59
v0.1.58
v0.1.57
v0.1.56
v0.1.55
v1.0.0.53
v1.0.0.52
v1.0.0.51

faming_hou@Azure:~$
```

## Delete an image in an Azure Container Registry

```console
az acr repository delete -n ${MyRegistry} --image addressservice:v1.0.0.51 --yes
```





