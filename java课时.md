# *内存管理和GC*

## 1.复制

满了触发,遍历内存空间,把需要的内存拷贝到另一个分区

暂态内存 快  没有缝隙  利用率底

java 对象模型

-------------------------------------

```
header 64bit

object

MyClass : 属性

索引->metaClass,方法, 静态属性
```

-------------------------------------------

## 2.标记清除

```
标记阶段: 清理时停止
去标记每一块没用的内存
将没用的内存用链表连起来
暂态内存,持久内存
会产生内存碎片
```

## 3.标记整理(Full GC)

```
把内存整理到一块
```

内存 年轻代minorge   老年代majorge

年轻代=>

eden   ->   s0   ->   s1

Serial => Stop The World

paralled

parnew form to eden 

老年代 =>

 CMS + full GC

gc=>stw=>{

|初始标记:stw------并发标记: not stw----重新标记: stw------清除: 改表|

}

G1 + full GC=>老年代和年轻代  但是内存不能超过64G

java -Xmx=64G -Xmn=32G -surv8 -cp xxx.jar org.xxx.Main

-surv8 eden..

Xmx老年

Xmn年轻

G1 {

​	分格存储

​	region

}

当把一个年轻代变成老年代时,年轻代引用年轻代时用一个card table 记录

三色标记优化 snapshot

在并发标记时修改时存表

cs可回收的内存链表

后端: 业务开发  中间件  存储  基础软件  测试开发=>selenuim postman jmeter ) 

运维开发  端基础研发=>嵌入式研发=>物联网



不垃圾回收

内存逃逸  如果不逃逸,内存在栈,而且如果有锁,则去锁, 反之在堆  

堵塞=>中断唤醒(长时间睡眠)  死循环改变条件(短时间睡眠)

重量级=>pthread.so=>pthread_mutex_lock()

x86_64 => cpu => 内存

占用总线

numactl --hardware

缓冲行 64B

拿数据 通知其他  如果有变为Share ,要修改时通知另一个  另一个将正常改为脏  

intel 写在读前 



继承 依赖 组合

java  c++   python

句柄

### 反射

类加载 

readelf -h ~/a

Meta Space=>静态区,方法区,静态表键值对,只读区(常量池String)



```java
Class<T1> t1cl = Class.forName(...);
Constructor con = t1cl.getDeclaredConstructor(int.class);


Class t1cl = Class.forName(...);
Constructor con = t1cl.getDeclaredConstructor(int.class);//代表有几个参数
Object t1 = con.newInstance(1);
t1.getDeclaredMethod("xxx", int.class).var;
t1c1.get.Filed().var;
var.Acc.(true);
var.set(t1,10);
var.get();

```

MySQL

show xxx

show create xxx

注释/* */ #

扩展语法/*! xxx */

id用 varchar() 可以是 2024_11_21_00000001

```mysql
create table user(id int, name varchar(20), password varchar(20), birth int, primary key(id))engine=innodb default charset=utf8;
```

desc tablename;

system cls;





动态实现逻辑=>动态流程变更     动态功能修改 

程序=>被编译的代码=>解释执行(这里的代码不被编译)=>被编译代码                    -----脚本,实现动态流程变更

动态功能修改: 工作流程不是全的, 在工作时选择一个被编译的代码放进去

动态功能 => osgi

方法区:class{}..  method{} 

虚拟机读class文件, 建表 : 类名 = > 字节码地址

把需要的字节码放入字节码栈

把字节码编译后放入机器码栈, 让CPU执行

cpu单独控制的空间=> non-shared

线程优先级 => 亲和性(这是动态的)

缓存行 64B

用户态切换到内核态

1.陷阱门

​	a.异常自陷 无法屏蔽 重新执行触发中断的语句

​	b.软件触发 不重新执行触发中断的语句

2.中断门

​	a.硬件触发

3.任务门

4.调用门

内核四个对象 dentry=>path inode=>硬连接  file superblock

PMA 协处理器    				0拷贝

## 25/1/13 图计算

分类 > 数字量

回归 > 模拟量

模式识别（ocr）、机器学习、

# 搜索

稀疏搜索
密集搜索
倒排索引

词频和逆频

一个词在一个文章中出现的次数, 越多越好

一个词在一群文章出现的次数(1个文章算一次), 越少越好

```hive
TermRangeQuery
PrefixQuery
wildcardQuery
=============
RegexpQuery 正则
FuzzyQuery
PhraseQuery 一个条件搜索多列
stringquery
matchquery 分词搜索分词==>>不支持表达式
```

```markdown
MUST 与
should 或 影响词权
MUST_NOT 非
Filter 或 不影响
```

组播UDP



WAP_x_PSK 代表简单认证
WEP 可以不设置密码
WAPI 国产,兼容性差
