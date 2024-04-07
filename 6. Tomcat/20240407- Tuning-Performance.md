#### The Maximum Number of Open Files on Linux
Check current config
```yml
ulimit -n
```
Edit: vim /etc/sysctl.conf
```yml
fs.nr_open = 1200000
fs.file-max = 200000
```
vi /etc/security/limits.conf
```yml
* soft nofile 128000
* hard nofile 128000
```

#### Increase Outgoing Network Sockets Range
Check current config
```yml
sysctl net.ipv4.ip_local_port_range
```
Edit: vim /etc/sysctl.conf
```yml
net.ipv4.ip_local_port_range = 1024 65535
```
