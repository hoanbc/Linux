### Edit file below for enable x-forwarded-for
vim /opt/tomcat/conf/server.xml
```yml
        <Valve className="org.apache.catalina.valves.RemoteIpValve"
                internalProxies="10.200.12.233"
                remoteIpHeader="x-forwarded-for"
                protocolHeader="x-forwarded-proto"
                proxiesHeader="x-forwarded-by"
        />
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               requestAttributesEnabled="true"
               prefix="access_log" suffix=".log"
               pattern="%h %l %u %t &quot;%r&quot; %s %b"
        />
```
