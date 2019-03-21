### Uninstall old versions

```console
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Update the apt package index

```console
sudo apt-get update
```

### Install packages to allow apt to use a repository over HTTPS:

```console
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

### Add Docker’s official GPG key:

```console
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### Verify the fingerprint

Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.

```console
sudo apt-key fingerprint 0EBFCD88
    
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

### Set up the stable repository

```console
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### Install a specific version of Docker CE
  a. List the versions available in your repo:
```console
apt-cache madison docker-ce

  docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.1~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  docker-ce | 18.06.0~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
  ...
```
  b. Install a specific version using the version string from the second column, for example, 18.06.1~ce~3-0~ubuntu. 
```console
sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu docker-ce-cli=18.06.1~ce~3-0~ubuntu containerd.io
```

```console
sudo service docker status
```


### The user jenkins needs to be added to the group docker

```console
sudo usermod -a -G docker jenkins
sudo service jenkins restart
```

### References
[Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)