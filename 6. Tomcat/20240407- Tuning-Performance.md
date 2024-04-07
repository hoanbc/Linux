#### The Maximum Number of Open Files on Linux
vim /etc/sysctl.conf
```yml
fs.nr_open = 1200000
fs.file-max = 200000
```
vi /etc/security/limits.conf
```yml
* soft nofile 128000
* hard nofile 128000
```
