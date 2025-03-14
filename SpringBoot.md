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

项目启动时进行初始化的注解

```java
@PostConstruct
public void initSpring(){
    ...
}
```

配置类注解

```java
@Configuration // @SprintBootConfiguration 一样
public class AppConfig{
    @Scope("prototype") // 表示这个类是多实例的
    @Bean("user") // 组件在容器中的名字
    public User user(){
        User u = new User();
        u.setId(1);
        u.setName("XXX");
        return u;
    }
}
// 条件注解
@ConditionalOnClass //如果存在这个类,就干什么
```

日志

```java
Logger log = LoggerFactory.getLogger(getClass());
log.info("info日志");
// @Slf4j 会自动给属性注入log对象
import lombok.extern.slf4j.Slf4j;
@Slf4j
@Controller
public class IndexController {
    @RequestMapping("/test")
    public String test(Model model){
        log.info("有用户上线了");
        model.addAttribute("msg", "I am MSG");
        model.addAttribute("users", Arrays.asList("sayo","soyo","saya"));
        return "test";
    }
}
// 可以在配置文件里配置类的日志级别

// 日志打印值
log.info("A:{},B:{}",a,b);
```

```yaml
# 日志可以分组
logging:
  pattern:
    console: "%clr(%d{yyyy-MM-dd HH:mm:ss}){faint} %clr(%-5level) %clr([%thread]){faint} %clr(%logger{15}){cyan} %clr(:){faint} %magenta(%C{1}) %clr(:){faint} %magenta(%L) ===> %msg%n"
  level:
    root: info
    com.bainai.controller: debug
  group:
    MyGroupName:
      com.bainai.controller,com.bainai.dao
```

