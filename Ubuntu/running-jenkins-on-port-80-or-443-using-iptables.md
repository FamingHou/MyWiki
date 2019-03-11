### Prerequisites ###

```console
iptables -L -n
```

If you don't see entries for these ports, then you need to run commands (as root or with sudo) to add those ports. For example, if you see none of these and need to add them all, you would need to issue the following commands:

```console

sudo iptables -I INPUT 1 -p tcp --dport 8443 -j ACCEPT
sudo iptables -I INPUT 1 -p tcp --dport 8080 -j ACCEPT
sudo iptables -I INPUT 1 -p tcp --dport 443 -j ACCEPT
sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT
```
### Forwarding ###

Once the before and after forwarding ports are allowed, you can now run the command to forward port 80 traffic to 8080, and port 443 traffic to 8443. The commands look like this:

```console
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8443
```

### Saving iptables Configuration ###

Using the iptables command to change port configuration and routing rules only changes the current, in-memory configuration. It does not persist between restarts of the iptables service. So, you need to make sure you save the configuration to make the changes permanent.

On a Debian-based system (Debian, Ubuntu, Mint, etc), issue the following command:

```console
sudo sh -c "iptables-save > /etc/iptables.rules"

```

[Running Jenkins on Port 80 or 443 using iptables](https://wiki.jenkins.io/display/JENKINS/Running+Jenkins+on+Port+80+or+443+using+iptables)