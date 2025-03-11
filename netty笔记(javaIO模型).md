



传统的BIO /堵塞IO

NIO的服务器编写

```java
package Server;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class NIOTest {
    public static void main(String[] args) throws Exception {
        new NIOTest().NioTest();
    }

    private void NioTest() throws Exception {
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        //设置为NIO
        serverSocketChannel.configureBlocking(false);
        //绑定端口号
        serverSocketChannel.bind(new InetSocketAddress(9999));
        List<SocketChannel> channels = new CopyOnWriteArrayList<>();
        while (true) {
            SocketChannel socketChannel = serverSocketChannel.accept();
            if (socketChannel != null) {
                System.out.println("one client connect!(" + (channels.size() + 1) + "个)");
                //设置socket为NIO
                socketChannel.configureBlocking(false);
                channels.add(socketChannel);
            }
            for (SocketChannel channel : channels) {
                try {
                    ByteBuffer buffer = ByteBuffer.allocate(128);
                    int len = channel.read(buffer);
                    if (len > 0) {
                        System.out.print("收到:" + new String(buffer.array(),0,len));
                    }
                } catch (IOException e) {
                    //客户端断开连接
                    channels.remove(channel);
                    System.out.println("有一个客户端断开连接!(" + channels.size() + "个)");
                }
            }
        }
    }
}

```

传统BIO服务器代码 --- 有一个请求起一个线程,优化:使用线程池减少消耗

```java
package Server;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class BIOTest {
    public static void main(String[] args) throws Exception {
        new BIOTest().BioTest();
    }

    public void BioTest() throws Exception {
        ServerSocket ss = new ServerSocket(9999);
        while (true){
            Socket accept = ss.accept();
            new Thread(() -> message(accept)).start();
        }
    }

    private static void message(Socket accept) {
        try {
            System.out.println("one client connect!");
            InputStream inputStream = accept.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
            while (true){
                String line = br.readLine();
                if (line == null) {
                    break;
                }
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("the client closed");
        }
    }
}
```





普通的NIO

字节缓冲区使用

```java
FileChannel chan = new FileInputStream("data.txt").getChannel();
ByteBuffer buffer = ByteBuffer.allocate(10);//申请缓冲区大小为10
while(true){
    int len = chan.read(buffer);//channel往缓冲区内读取数据
    if(len < 0){
        break;
    }
    buffer.flip();// 切换读模式
    while (buffer.hasRemaining()){ //是否还有未读的数据
        byte b = buffer.get();
        sout((char) b);
    }
    buffer.clear();// 切换为写模式
}
```

ByteBuffer:

capacity容量

position位置

limit限制

buffer:存取

channel:数据通信

# Buffer使用

Buffer的获取f

```java
static XxxBuffer allocate(int capacity);//创建一个容量为cap的buffer对象
```

Buffer常用方法

```java
Buffer clear();//情空缓冲区并返回缓冲区----读完了,切换为写
Buffer flip();//将起始位置设置为0----切换为读
boolean hasRemaining();//判断缓冲区内是否还有元素
get();//拿
put();//放
```

字符集类

1.Charset

2.StandardCharsets

```java
BF bf = CharSet.xxxSet().encode("str");
//通过这些方法放buffer会自动转成读模式
BF bf= StanderdCharsts.UTF_8.encode("hello");
Bf bf ByteBuffer.wrap("ss".getBytes());
//转换成str
Standar..UTF_8.decode(buffer).toString();
```

重新读

```
buffer.rewind();
```

Mark & Reset

mark保存当前position,reset将position跳转到mark



将buffer中未读的数据移到开头并切换为写模式

```
buffer.compact();
```

Selector

```java
Selector s = Selector.open();
//将管道注册到选择器上
channel.register(s,监听的事件时间);
/*value:
读->1->SelectionKey.OP_READ
写->4
连接->8
接受->16
*/
```

有selector的NIO服务器

```java
    public static void main() throws Exception {
        Selector selector = Selector.open();
        ServerSocketChannel ssc = ServerSocketChannel.open();
        ssc.configureBlocking(false);
        ssc.bind(new InetSocketAddress(9999));
        ssc.register(selector, SelectionKey.OP_ACCEPT);
        while (true) {
            selector.select();
            Set<SelectionKey> keys = selector.selectedKeys();
            Iterator<SelectionKey> it = keys.iterator();
            while (it.hasNext()) {
                SelectionKey key = it.next();
                it.remove();
                if (key.isAcceptable()) {
                    System.out.println("连接成功!");
                    SocketChannel sc = ssc.accept();
                    sc.configureBlocking(false);
                    sc.register(selector, SelectionKey.OP_READ);
                } else if (key.isReadable()) {
                    try {
                        ByteBuffer buffer = ByteBuffer.allocate(10);
                        SocketChannel channel = (SocketChannel) key.channel();
                        int len = channel.read(buffer);
                        if (len > 0) {
                            buffer.flip();
                            System.out.print("say:" + new String(buffer.array(), 0, len));
                            buffer.clear();
                        } else if (len == -1) {
                            //客户端正常退出
                            key.cancel();
                            key.channel().close();
                        }
                    } catch (IOException e) {
                        //客户端异常退出
                        key.cancel();
                        System.out.println("下机了");
                    }
                }
            }
        }
    }
```

selector没有事件时会堵塞,有事件不会堵塞

selector.select();会把监听的key中发生事件的key放进selectorKeys中

socket断开时会发生READ事件

3种传输方式

1.定长传输

2.分隔符传输

3.数据前有一个大小值

注册时添加附件

```java
sc.register(selector,监听事件类型,buffer);
//获取附件
key.attachment();
//替换附件
key.attach(newBuffer);
//添加事件
key.interestOps();
```

Netty

EventLoopGroup

事件循环组
