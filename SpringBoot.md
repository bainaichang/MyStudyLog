springboot的多环境配置

```yaml
server:
	port: 8081
spring:
	profiles:
		active: dev
---
server:
	port: 8082
spring:
	profiles: dev
---
server:
	port: 8083
spring:
	profiles: test
```

springboot通过yaml配置类的默认值

```java
@ConfigurationProperties(prefix = "student")
public class Student{
    ...
}
```

```yaml
student:
	name: xxx
	id: 1001
```

templates为模版包,只能通过controller访问

需要导入Thymeleaf引擎

```xml
<!-- 引入Thymeleaf模板引擎 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

模版

```html
<html xmlns:th="http://www.thymeleaf.org">
    
</html>
```

bootstarap 模版