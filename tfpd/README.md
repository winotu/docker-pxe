# Overview
```
TFTP Server               TFTP Client

IF: enp0s8                IF: enp0s3
+--------------+          +---------------+
|              |          |               |
| 192.168.2.1  +----------+  192.168.2.3  |
|              |          |               |
+--------------+          +---------------+
```

# Requriements
 Firewall rule added for port 69 udp 

```
---------- On CentOS/RHEL 7 ----------
# firewall-cmd --add-service=tftp --permanent 
# firewall-cmd --reload 

---------- On CentOS/RHEL 6 ----------
# iptables -A INPUT -p tcp -m state --state NEW --dport 69 -j ACCEPT
# service iptables save
```

# Run
```
docker build -t pxe-tftp:latest .
docker run -d -p 192.168.2.1:69:69/udp -v $(pwd)/<yourbootfiles>/:/var/lib/tftpboot/ --name pxe-t pxe-tftp

```

# debugging
Test fetch
```
curl tftp://192.168.2.1/pxelinux.0
```

Check listening port
```
netstat -ln | grep 69
```

Logs from container
```
docker run -d --net host -v $(pwd)/fs/:/var/lib/tftpboot/ --name test_t --entrypoint /bin/bash tftp_d -c '/usr/sbin/rsyslogd && /usr/sbin/in.tftpd -vvv -L -s /var/lib/tftpboot'
docker exec test_t cat /var/log/messages
```
# Sources
Debugging with syslog in tftp

https://www.projectatomic.io/blog/2014/09/running-syslog-within-a-docker-container/
