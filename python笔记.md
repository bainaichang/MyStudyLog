```python
r'' #不转义
```



Python使用抽象方法 

属性开头用'__'表示该属性为私有

```python
@abc.abstractmethod
def xxx(self):
	pass
```

迭代器

```python
#__iter__ 和 __next__
class person():
	i=0
    def __iter__(self):
        return self
    def __next__(self):
        if self.i < 10:
            self.i += 1
            return self.i
p=person()
ptr=iter(p)
print(next(ptr))
print(next(ptr))
#结果:
# 1
# 2
```

for利用迭代器

```py
class person():
	i=0
    def __iter__(self):
        return self
    def __next__(self):
        if self.i < 10:
            self.i += 1
            return self.i
        else:
            raise StopIteration
p=person()
ptr=iter(p)
for i in ptr:
    print(i)
#结果:1到10
```

Python继承:混入式

```
class C(B,A)
//重新生成一个新类,重复的东西不会放进去,解决了多义性
```

单继承:

```c
struct A a{}
//A->B
struct B b{
    struct A a;//依赖关系是 *a,指针
}
```

C++继承:(基于内存构造的多继承) 虚表实现

```
Class B: virtual class A{
	//A->B
	void *ptr;//指向一个哈希表,key:类名,value:类对象的地址
	用于解决多继承导致的内存重复问题
}
```

堆叠式继承:

将继承的类放在最后:prototype是一个指向表的指针,在最后

使用类时从上往下找第一个匹配的东西

```
Python中使用__mro__ 返回所有继承关系的元组,有object
__bases__返回所以继承关系的元组,没有obj,可以修改继承关系
```

type与object的本质

在cpython下有元类

```
struct object{}
strucct type{
	struct object
}
```

而在py runtime下obj参考cpy中的obj申请内存,type参考obj+type的内存,所以type继承obj

而py runtime里的type下有一个小type,所以当用type查看type时返回type,看不到cpy中的所有东西

通过gc.getx..(类)可以获取一个数组,数组第一个元素是关于类的字典,可以直接修改类添加属性

面向对象语法解析:

p=Person()

语法糖

p.dis() => Person.dis(p)

运算符重载

```py
#重载了+运算符
(o1+o2).dis() => (o1.__add__(o2)).dis()
```

小括号运算符为--call--()

```python
def __call__(self, f):
	
```

get->@property

```python
class person():
	@property
	def xx(self):
		return self.__xx
	@xx.setter
	def xx(self,name):
		self.__xx=name
```

单例模式

@classmethod

```python
@classmethod
def my_new(cls):
	return cls()
```

##### 元类

通过type定义类就是元类

```
type("Person"//类名, (object, )//父类,{"id":0,"name":"xxx","__init__":func//构造函数}//属性)
```

```
//另一种定义元类的方法
class Person(object, metaclass=type):
//编译器会把该语法转换成上面的格式
//此方法为语法糖
```

type可以通过参数作为模版定义类

new__可以相当于创建内存时使用的方法

自定义metaclass

```py
class My_type(type):
#type('classname', (parent), {'k':v,'k2':v2})
	def __new__(cls, classname, parent, attr)
		clz=super(cls,cls).__new__(cls,classname,parent,attr)
		return clz
#语法糖,会变成->My_type(classname, (parent), {'k':v,'k2':v2})
```

通过这种方法可以自定义定义类,方便写框架,能自动给类添加属性

当继承了type时,class.--bases--只会有type,没有obj,就是元类

创建对象时要传入3个参数

```py
p = My_abc("My_abc", (object, ), {"i":0,})
```



```py
class My_type(type):
#type('classname', (parent), {'k':v,'k2':v2})
	def __new__(cls, classname, parent, attr)
		attr["自动添加的属性"] = value
		clz=super(cls,cls).__new__(cls,classname,parent,attr)
		return clz
#作业实现抽象类
class MYABC(type):
    def __new__(cls, classname, parent, atrr):
        res = super().__new__(cls, classname, parent, atrr)
        print("res=", res)
        res.__abstractmethods__="aaa"
        return res;
```

实现抽象类的底层

```py
class B:
    pass
B.__abstractmethods__={"xxx", "yyy"}
b = B()
#TypeError: Can't instantiate abstract class B without an implementation for abstract methods 'xxx', 'yyy'
#只要该属性不为空就是抽象类
```

除了基本类型(int,...,集合,list,turpl,set,....)之外,都可以通过' . '运算符添加属性

装饰器

```py
def outter(f):
    def inner():
        print("in")
        f()
        print("out")
    return inner
#---------------------------------------------
@outter #转换成xxx=outter(xxx)
def xxx():
    print("xxx")
xxx()
```

@outter为语法糖,将下面的格式转换成上面代码

用于判断一个对象是否有某个属性,setattr用于添加属性

```
 if not hasattr(p,'age'):
     setattr(p,'age',18)
```

给对象添加方法

```
import types
class Person(object):
    def __init__(self,name):
        self.name = name
 
def run(self):
    print('%s在奔跑' % self.name)
 
p1 = Person('p1')
p1.run = types.MethodType(run,p1)
 
p1.run()
```

--slote-- 用于限制对象的成员

当代码想添加成员时会报错

```py
class A:
	__slots__ = ["init",'dis','id']
    #	__slots__ = ["init",'dis','id','__dict__'] 当加入dict这句话跟没写一样
```

##### 反射

```py
class A:
	__a=1
getattr(A,"_A__a")
setattr(A,"_A__a", "2")
hasattr(A,"_A__a")#True
```

globals()返回模块的键值对

del 去掉标记

```py
a=1
del a
print(a)
#undefine a
```

callable()函数,用于判断一个对象是否可以调用(是否为一个函数)

模块

有--init--.py就是模块

会执行包里的代码

--init--

```
__all__ = ['txt1']#导入模块
```

装饰器

```py
def myxx(name):
    def _(fun):
        def _():
            print('in')
            fun()
            print('out')
         return _
    return _
@myxx(name="YYXX")#test=myxx(name="YYXX")(test)
def test():
    print('test')

test()
```

传参数的装饰器

```py
def myxx(name):
    def _(fun):
        def _(b):
            print('in')
            fun(name, b)
            print('out')
         return _
    return _
@myxx(name="YYXX")#test=myxx(name="YYXX")(test)
def test(name, b):
    print('test',name,b)

test("Xxx")#此时test 为最里面的_()函数,name参数是myxx()传进去的,而b参数是最里面
#的函数的参数,通过此句话的"Xxx"传入
```

静态方法如何写

```py
#使用@staticmethod
class A:
	@staticmethod
	def xxx():
		pass
```

#### IO

字符io

字节流B, 字符流



如果大小0~127就拷贝

utf-8 -> 1~4byte

第一位 

例子,假设

-1 -> 2B, -2 -> 3B

http1.1, request -> content-type {  text, appliction, multi }:

```
常见的媒体格式类型如下：

text/html ： HTML格式
text/plain ：纯文本格式
text/xml ： XML格式
image/gif ：gif图片格式
image/jpeg ：jpg图片格式
image/png：png图片格式
以application开头的媒体格式类型：

application/xhtml+xml ：XHTML格式
application/xml： XML数据格式
application/atom+xml ：Atom XML聚合格式
application/json： JSON数据格式
application/pdf：pdf格式
application/msword ： Word文档格式
application/octet-stream ： 二进制流数据（如常见的文件下载）
application/x-www-form-urlencoded ： <form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）
另外一种常见的媒体格式是上传文件之时使用的：

multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式
```

base64 --> 让3B+1=4B ,只有A-z, 0-9,/,+,=的可见字符

%E6 -> 对   应字符集的16进制, urlencoding 编码



文件打开

```py
with open(inpath, 'r', encoding='UTF-8') as src, open(out, 'w') as dest:
    t = src.read()
    dest.write(t)
    src.read(1)
```

字符流 遇到回车自动flush()

重定向 

```
sts.stdin=open(file)
print("aaa", file=newFile)
```

字节流

```
b=io.BytesIO(b'\xAA\xAV')
```

getopt



 多线程

1.进程 ->内核态task_struct linux 内核宏核, 没有进程 用户态=>模拟进程 分代码段()和数据段(写时复制)

2.线程->内存共享,

ppid=父id 

pid=自己的id

 tid = 线程id

tgid == pid

sid = 用户主id,会话组id

后台会话不属于任何一个组

守护进程

3.协程

进程:

```
Value b = ('b', 1)
Array a = ('i', range(11))
p = Process(target=func,args=("aaa",))
p.start()
p.join()
```

shell

```py
subprocess.run("echo aaa", shell=True)
p=subprocess.run('ls /', shell=True, stdout=subprocess.PIPE)
print(p.sudout.decode('utf-8'))
p=subprocess.run(['python3', './test.py'], input=b'password\n', stdout=subprocess.PIPE)

```

线程:

```
t1=threading.Thread(target=func)
t1.start()
```

condition()

threading.lcoal() #线程专属内存

协程:

corutinue, gorutinue

单线程

cpu=>scheduler 处理一个任务平衡二叉树, 节点叫task_stuct, 最左节点先执行

cpu<-apic可编程中断控制器-<rtr时钟

TCP

```python
s = socket.socket(socket.AF_INET,socket.SOCK_STEAM)

s.bind(("0.0.0.0", 10021))#0.0.0.0 全网段
s.listen(1)//最大连接数
//客户端
s=socket.socket(socket.AF_INET,socket.SOCK_STEAM)
s.connect(("127.0.0.1", 10021))
s.send(data.encode())
//UDP
socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

```

绕栈方法

1.socket->ringbuf

2.ebpf -> side work

3.dpdk intel ->driver remapping

4.rdma







类对象装饰器

```py
class decorate:
    def __init__(self):
        pass
    def __call__(self,fun):
        def _(self):
            print('-'*10)
            fun(self)
            print('-'*10)
        return _
class Test:
    @decorate() #->xxx=decorate()(xxx)
    def xxx(self):
        print('xxx')
    t=Test()
    t.xxx()
```







与c交互

```
from ctypes import *
import os

```

mysql

```
mysql, postgresql , oracle
```

```python
import pymysql
db = pymysql.connect("localhost", "username" ,"password", "databaseName");
cursor = db.cursor()
sql="select * form user"
try:
	cursor.execute(sql)
    res=cursor.fetchall()
    for row in res:
        (v1,v2,v3,v4) = row
except:
    ...
db.close()
```

修改

```python
sql='update xxx set age=0 where id=10'
try:
    cursor.execute(sql)
    db.commit()
except:
    db.rollback()
db.close()
```

python mysql 框架 sqlalchemy

```py
db_connect_string='mysql://username:password@localhost:3306/database?charset=utf8'
Base=declarative_base()
class Account(Base):
    __tablename__=u'account'
    id = Column(Integer,primary_key=True)
    user_name=Column(String(50),nullable=False)
```

 爬虫, ->lxml,beautifulsoap

```
键盘->[消息队列]<-桌面 -- list[active ] 
 			  <- XXX
```

```
import requests
heads={'key'='vaule'}
//post
params={
'key'='vaule'
}
res=requests.get("https://www.baidu.com",params headers=header)//把get改成post就是发post
res.text
```





soup=BeautifulSoup(res.text,'html.parser')

urls=selector.xpath('xpath')





Flask 

templates ->html

static -> css,js,jpg....

get post 上传  下载

request response 对象

生命周期:一次提交 浏览器打开到关闭session 浏览器cookie时长 网站从启动到结束gloable/application 

static css, js, ...

render_template('index.html'), 404

```
@app.errorhandler(404)
def err(error):
	return render_template('x.html')
@app.route('/s/')
@app.route('/s/<name>/<int:age>/')
def s(name='x', age=0)
```



app.add_url_rule(rule='/h', view_func=render_template('x.html'))

300 跳转新页面

```
@app.route('/red')
def red():
	return redirect('url')
@app.route("/l", methods=["POST"])
	request.stream.read().decode('utf-8')
	request.form.get('username')
	request.headers.get('key')
@app.route('/download/<filename>')
def download(filename):
	fi request.method =='get':
		return send_from_directory('文件夹名', filename, as_attachment=True)
makeresponse
//加盐
app.secret.key='abafb'
url_for('func') //把函数的请求路径打出来
```

模版

