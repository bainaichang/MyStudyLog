注册驱动

```java
Class.forName("com.mysql.jdbc.Driver");
```

获取链接

```java
String url = "jdbc:mysql://127.0.0.1:3306/database01";
        Connection conn = DriverManager.getConnection(url, "root", "password");
        Statement state = conn.createStatement();
        String sql1 = "select * from tb_stu";
        ResultSet rs = state.executeQuery(sql1);
```

