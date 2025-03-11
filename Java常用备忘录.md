输出当前工作目录

```java
System.getProperty("user.dir");
```

BufferedReader的使用

```java
BufferedReader br = new BufferedReader(new FileReader(FilePath));
```

java 使用命令行启动程序 假设类为Main.class,Package 没有

```sh
java Main
```

有的话就跟上包名.主类名  要注意命令行所处的目录,要在外面的主目录下

```
java org.XX.Main
```

运行jar包:    

```
java -cp XXX.jar org.XXX.Main
```

http GET 请求头后面2个"\n"表示结束

多线程安全List

```
CopyOnWriteArrayList
```

