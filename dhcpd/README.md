# Overview
```
DHCP Server               DHCP Client

IF: enp0s8                IF: enp0s3
+--------------+          +---------------+
|              |          |               |
| 192.168.2.1  +----------+  192.168.2.3  |
|              |          |               |
+--------------+          +---------------+
```

# Requriements
 if host IF has no IP assigned one can set it 

## one time
```
# ip a ...
# ifconfig
```
## ifcfg file
Example: /etc/sysconfig/network-scripts/ifcfg-enp0s8
```
TYPE=Ethernet
BOOTPROTO=static
ONBOOT=yes
DEVICE=enp0s8
IPADDR=192.168.2.1
NETMASK=255.255.255.0
GETWAY=192.168.2.1
```
set firewall rule for port 67/udp
```
---------- On CentOS/RHEL 7 ----------
# firewall-cmd --add-service=dhcp --permanent 
# firewall-cmd --reload 

---------- On CentOS/RHEL 6 ----------
# iptables -A INPUT -p tcp -m state --state NEW --dport 67 -j ACCEPT
# service iptables save
```

# Run
```
docker build -t pxe-dhcpd:latest .
docker run -d --net host pxe-dhcpd
```

# debugging
Check listening port
```
netstat -alnp | grep dhcpd
```

Logs from container
```
docker logs pxe-dhcpd

```

# TODO
- dhcpd user used to run dhcpd binary (maybe USER in Dockerfile)
- is separation of dhcpd.conf and .leases needed? PROS and CONS
- 

# Sources
LPF issue:
https://unix.stackexchange.com/a/301870

https://github.com/fedora-cloud/Fedora-Dockerfiles/tree/master/dhcpd

https://docs.fedoraproject.org/en-US/fedora/f29/install-guide/advanced/Network_based_Installations/
