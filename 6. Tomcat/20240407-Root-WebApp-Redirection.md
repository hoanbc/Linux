#### Config redirection on root webapp
vim /opt/tomcat/webapps/ROOT/index.jsp
```yml
<%
  final String redirectURL = "/sample/";
  response.setStatus(response.SC_MOVED_PERMANENTLY);
  response.sendRedirect(redirectURL);
%>
```
vim /opt/tomcat/webapps/ROOT/WEB-INF/web.xml
```yml
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
        http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
        version="3.0">
        <display-name>Redirect</display-name>

        <servlet>
                <servlet-name>index</servlet-name>
                <jsp-file>/index.jsp</jsp-file>
        </servlet>

        <servlet-mapping>
                <servlet-name>index</servlet-name>
                <url-pattern>/</url-pattern>
        </servlet-mapping>

        <error-page>
                <location>/index.jsp</location>
        </error-page>
</web-app>
```