### Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock

```console
sudo usermod -a -G docker jenkins
sudo service jenkins restart
```