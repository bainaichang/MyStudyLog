通过Maven配置tomcat

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
                <port>80</port>
                <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```

```java
@WebServlet(urlPatterns="/demo", loadOnStartup=1)
>=0时,启动时创建
else,第一次访问时创建
```

一个Servlet,可以配置多个urlPattern

```java
@WebServlet(urlPatterns={"/1", "/2"})
```

访问路径可以使用 *

```java
@WebServlet("*.do")
```

请求和响应

get方法获取参数

Map

```java
Map<K,V[]> map = req.getParameterMap();
```

通过Key获取数组

```java
String s[] = req.getParameterValues("Key");
```

通过Key获取单个值

```java
String s = req.getPatameter("Key");
```

POST方法通用

POST 解决中文乱码 修改req的Reader编码

```java
req.setCharacterEncoding("UTF-8");
```

URL编码:将一个字节用2个16进制数表示,%11%

get解决乱码问题

由于tomcat使用ISO-8859-1编码

所以现将字符串解码成字节,再把字节转成utf-8字符串

```java
byte[] bs = xxx.getBytes(StanderdCharsets.ISO-8859-1);
xxx = new String(bs, StanderdCharsets.UTF-8);
```

请求转发

```java
request.getRequestDispatcher("/要转发的路径").forward(request,response);
```

共享数据,生命周期:一次请求

```java
request.setAttribute(key, value);//设置
Object msg = request.getAttribute(key);//获取
removeAttribute(key);//删除
```

Response 响应

设置状态码

```java
setStatus(int sc);
```

设置头键值对

```java
setHeader(String name, String value);
```

响应体:

```java
PrintWriter getWriter();//获取字符输出流
ServletOutputStream getOutputSteam();//获取字节输出流
```

重定向(Redirect) , 302

响应头 location: xxx

```java
resp.setStatus(302);
resp.setHeader("Location", "/demo");
//简化
reponse.sendRedirect("/demo")
```

获取虚拟目录

```
request.getContextPath()
```

设置返回的文档类型

```
setHeader("content-type", "text/html");
//简化
response.setContentType("text/html;charset=utf-8");
```

IO工具类:

```
commons-io
IOUtils.copy(输入流, 输出流);
```

会话追踪技术

发送 cookie

```java
Cookie cookie = new Cookie("key", "value");
response.addCookie(cookie);
```

获取cookie

```java
Cookie[] cookies = request.getCookies();
cookie.getName();
cookie.getValue();
```

cookie存活时间

```java
setMaxAge(int seconds);
```

cookie存中文

```java
//使用url转码
String v="我";
v = URLEncoder.encode(v,"UTF-8");
Cookie c = new Cookie("key", v);
//url解码
s = URLDecoder.decode(v, "UTF-8");
```

Session

获取session

```java
HttpSession session = request.getSession();
void setAttribute(String name ,Object o);//存储数据到session域中
Object getAttribute(String name);//根据key返回值
void removerAttribute(String name)//删除
```

销毁

```
session.invalidate();
```

Filter拦截器

```
实现Filter接口
@WebFilter("/*")//拦截路径
放行
chain.doFilter(req,resp);
```

监听器

ServletContextListener

@WebListener
