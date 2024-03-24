### Create user tomcat and cicuser
```yml
useradd -m -d /opt/tomcat -s /usr/sbin/nologin  tomcat

useradd cicuser
sudo usermod -a -G systemd-journal cicuser
sudo usermod -a -G tomcat cicuser
```
### Allow cicuser for start and stop services
```yml
cicuser         ALL=(ALL)       NOPASSWD: /bin/systemctl stop tomcat
cicuser         ALL=(ALL)       NOPASSWD: /bin/systemctl start tomcat
cicuser         ALL=(ALL)       NOPASSWD: /bin/systemctl status tomcat
```
### Install java 11 jdk
```yml
sudo yum install java-11-openjdk -y
```
### Check java version and java home
```yml
java -version
readlink -f $(which java)
```
### Download tomcat
```yml
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.19/bin/apache-tomcat-10.1.19.tar.gz
tar -xzvf apache-tomcat-10.1.19.tar.gz
cd apache-tomcat-10.1.19
mv * /opt/tomcat/
chmod 770 /opt/tomcat
chown -R tomcat:tomcat /opt/tomcat/
cd /opt/tomcat
rm -rf .bash* BUILDING.txt CONTRIBUTING.md R* LICENSE NOTICE
```
### Create service for tomcat 
vim /etc/systemd/system/tomcat.service
```ynk
[Unit]
Description=Tomcat 10 servlet container with port 8080
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat
Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.22.0.7-2.el8.x86_64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms2048M -Xmx4096M -server -XX:+UseParallelGC"
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```
### Start service
```yml
systemctl daemon-reload
sudo systemctl enable --now tomcat
```
### Open firewall
```yml
firewall-cmd --add-port=8080/tcp --permanent && firewall-cmd --reload
```
