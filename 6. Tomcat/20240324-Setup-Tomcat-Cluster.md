### Edit on node tomcat1
vim /opt/tomcat/conf/server.xml
```yml
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->
       <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
                 channelSendOptions="8">

          <Manager className="org.apache.catalina.ha.session.DeltaManager"
                   expireSessionsOnShutdown="false"
                   notifyListenersOnReplication="true"/>

          <Channel className="org.apache.catalina.tribes.group.GroupChannel">
            <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                      address="10.200.12.236"
                      port="4000"
                      autoBind="100"
                      selectorTimeout="5000"
                      maxThreads="6"/>
            <Membership className="org.apache.catalina.tribes.membership.StaticMembershipService">
              <Member className="org.apache.catalina.tribes.membership.StaticMember"
                          port="4000"
                          host="10.200.12.236"
                          uniqueId="{1,0,2,0,0,1,2,2,3,6,0,0,0,0,0,0}" />
              <Member className="org.apache.catalina.tribes.membership.StaticMember"
                          port="4000"
                          host="10.200.12.237"
                          uniqueId="{1,0,2,0,0,1,2,2,3,7,0,0,0,0,0,0}" />
            </Membership>
            <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
              <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
            </Sender>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatchInterceptor"/>
          </Channel>
          <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
                 filter=""/>
          <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
          <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
                    tempDir="/tmp/war-temp/"
                    deployDir="/tmp/war-deploy/"
                    watchDir="/tmp/war-listen/"
                    watchEnabled="false"/>
          <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
        </Cluster>
      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
```
### Edit on node tomcat2
vim /opt/tomcat/conf/server.xml
```yml
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->
       <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
                 channelSendOptions="8">

          <Manager className="org.apache.catalina.ha.session.DeltaManager"
                   expireSessionsOnShutdown="false"
                   notifyListenersOnReplication="true"/>

          <Channel className="org.apache.catalina.tribes.group.GroupChannel">
            <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                      address="10.200.12.237"
                      port="4000"
                      autoBind="100"
                      selectorTimeout="5000"
                      maxThreads="6"/>
            <Membership className="org.apache.catalina.tribes.membership.StaticMembershipService">
              <Member className="org.apache.catalina.tribes.membership.StaticMember"
                          port="4000"
                          host="10.200.12.236"
                          uniqueId="{1,0,2,0,0,1,2,2,3,6,0,0,0,0,0,0}" />
              <Member className="org.apache.catalina.tribes.membership.StaticMember"
                          port="4000"
                          host="10.200.12.237"
                          uniqueId="{1,0,2,0,0,1,2,2,3,7,0,0,0,0,0,0}" />
            </Membership>
            <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
              <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
            </Sender>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatchInterceptor"/>
          </Channel>
          <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
                 filter=""/>
          <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
          <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
                    tempDir="/tmp/war-temp/"
                    deployDir="/tmp/war-deploy/"
                    watchDir="/tmp/war-listen/"
                    watchEnabled="false"/>
          <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
        </Cluster>
      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
```
### Restart service on both node
systemctl restart tomcat
