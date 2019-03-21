### Installing Open JDK 8

```console
sudo apt-get install openjdk-8-jdk -y
```

### Installing Jenkins on Ubuntu

```console
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins -y
```

### Using iptables for port 80 -> 8080

```console
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8443
sudo iptables-save > /etc/iptables.rules
sudo apt-get install  iptables-persistent -y
```

