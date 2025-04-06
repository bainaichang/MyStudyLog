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

maven把依赖包打进去

```xml

<build>
    <plugins>
      <!-- Maven Shade Plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.0</version>
        <configuration>
          <createDependencyReducedPom>true</createDependencyReducedPom>
        </configuration>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                  <!-- 主类的全限定名 -->
                  <mainClass>xxx.Main</mainClass>
                </transformer>
              </transformers>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

java (命令行)常用检测工具

```shell
watch -d 'jmap -histo `pgrep java`|head' #查看java内存对象
jmap -histo `pgrep java`|head|numfmt --to=iec --invalid=ignore --field=3 #用便于查看的方式查看内存
pgrep java # 查找java的进程号
pgrep -f Main # 通过完整查找进程名找到进程号
jstat -gc `pgrep java` #查看这个java进程的垃圾回收情况
######
# S0C：第一个幸存区的大小
# S1C：第二个幸存区的大小
# S0U：第一个幸存区的使用大小
# S1U：第二个幸存区的使用大小
# EC：伊甸园区的大小
# EU：伊甸园区的使用大小
# OC：老年代大小
# OU：老年代使用大小
# MC：方法区大小
# MU：方法区使用大小
# CCSC:压缩类空间大小
# CCSU:压缩类空间使用大小
# YGC：年轻代垃圾回收次数
# YGCT：年轻代垃圾回收消耗时间
# FGC：老年代垃圾回收次数
# FGCT：老年代垃圾回收消耗时间
# GCT：垃圾回收消耗总时间
#######
jq . #格式化json
echo -n 你好|iconv -t ucs-2be|xxd -ps|sed 's/..../\\u&/g' # 将'你好'转成utf-16编码
echo -n "你好" | xxd -ps | sed 's/\(..\)/\\x\1/g' # utf-8
echo -e '\u4f60\u597d' # 解码
lsof -i:8888 # 查看该端口使用情况
pwdx `pgrep java` # 查看进程的目录
```

