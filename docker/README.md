# Docker

## Network configuration

### Check **/etc/docker/daemon.json**

* Disable legacy docker0 bridge
* Disable iptables & ip6tables 

### Check **/etc/systemd/network/default.network**

* Enable IP forwarding 
* Docker bridge et veth interfaces must be unmanaged by **systemd-networkd**

```
sudo networkctl 
IDX LINK        TYPE     OPERATIONAL SETUP     
  1 lo          loopback carrier     unmanaged
  2 enp7s0      ether    routable    configured
  3 dockervnet  bridge   routable    unmanaged
  5 veth91fbf6e ether    enslaved    unmanaged
```

### Create docker network

```
sudo docker network create -d bridge \
  --subnet=172.18.0.0/16 \
  --opt com.docker.network.bridge.host_binding_ipv4=172.18.0.1 \
  --opt com.docker.network.bridge.name=dockervnet \
  --opt com.docker.network.bridge.enable_icc=true \
  --opt com.docker.network.bridge.enable_ip_masquerade=true \
  dockervnet
```

### Setup nftables ruleset

* filter/FORWARD for opening flows from Docker
* nat/POSTROUTING for source masquerade

### Test

```
sudo docker run -it --network dockervnet --name c1 alpine:3.15
/ # ping www.google.fr
```