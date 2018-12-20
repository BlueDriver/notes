# Tomcat

## 配置

### 配置多个域名

在server.xml添加：

```xml
<Host name="www.qwer.com" appBase="webapps" unpackWARs="true" autoDeploy="true">	
	<Alias>qwer.com</Alias>	<!-- 别名 -->
	<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs" prefix="localhost_access_log" suffix=".txt" pattern="%h %l %u %t &quot;%r&quot; %s %b" />     
</Host>
```
然后重启Tomcat，会在Catlina下多一个www.qwer.com的文件夹，在其下配置project.xml即可