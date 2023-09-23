#### Download package
sudo yum install gcc wget
yum group install "Development Tools"
sudo wget http://www.squid-cache.org/Versions/v....
yum group install "Development Tools"
#### Extract & Compile
sudo tar -xvzf squid-6.0.0-20210929-re0072b1d2.tar.gz
sudo cd squid-6.0.0-20210929-re0072b1d2

#### Compile Settings
./configure --prefix=/usr --exec-prefix=${prefix} --bindir=${prefix}/bin --sbindir=${prefix}/sbin --libdir=${prefix}/lib --includedir=${prefix}/include --localstatedir=/var --libexecdir=${prefix}/lib/squid --srcdir=. --datadir=${prefix}/share/squid --sysconfdir=/etc/squid --infodir=${prefix}/share/info --mandir=${prefix}/share/man --x-includes=${prefix}/include --x-libraries=${prefix}/lib  --with-default-user=proxy --with-logdir=/var/log/squid --with-swapdir=/var/spool/squid --with-pidfile=/var/run/squid.pid --with-large-files --enable-silent-rules --enable-ssl --disable-inlined --disable-optimizations --disable-wccp --disable-wccp2 --disable-htcp --with-pthreads --with-openssl --enable-ssl-crtd --with-libnetfilter-conntrack --enable-storeio=ufs,aufs,diskd,rock --with-aufs-threads=64 --enable-linux-netfilter --enable-removal-policies=lru,heap --enable-gnuregex --enable-snmp --enable-inline --enable-htcp --enable-build-info=Ubuntu/linux --enable-x-forwarded-for --enable-x-accelerator-vary --enable-underscores --enable-http-violations --enable-async-io=64 --enable-storeid-rewrite-helpers --enable-auth-basic=DB,fake,getpwnam,NCSA,POP3,RADIUS,SMB --enable-arp --enable-arp-acl --enable-truncate --enable-stacktrace --enable-cache-digests --enable-icap-client --disable-ident-lookups --enable-delay-pools --enable-useragent-log --enable-disk-io="AIO,Blocking,DiskThreads,IpcIo,Mmapped" --disable-translation --disable-auto-locale && echo -e "CONFIGURATION SUCCESSFUL"

#### Make & Install 
sudo make && echo "MAKE CONFIGURATION"
sudo make install && echo "INSTALL SUCCESSFUL" 

#### Now that Squid is installed you need to set some permissions:
chown -R proxy:proxy /etc/squid /var/log/squid

#### Now in order to get Squid to start at boot do this:
cp ./tools/systemd/squid.service /etc/systemd/system/
systemctl enable squid
systemctl daemon-reload
service squid start | stop | restart | status 
