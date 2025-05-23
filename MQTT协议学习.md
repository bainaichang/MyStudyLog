**MQTT**协议数据包结构

[MQTT协议详解(完整版)-CSDN博客](https://blog.csdn.net/jackwmj12/article/details/129163012)

【尚硅谷MQTT协议教程，搭建mqtt服务器，物联网实战打通后端和嵌入式】https://www.bilibili.com/video/BV1HADnYFEXn?p=8&vd_source=0a43e748e352549cf44e318a53a70c6a

(视频好理解)

在MQTT协议中，一个MQTT数据包由: 固定头(Fixed header)、可变头(Variable header)、消息体(payload)三部分构成。

1. **固定头**(Fixed header)。存在于所有MQTT数据包中，表示数据包类型及数据包的分组类标识，如连接，发布，订阅，心跳等。其中固定头是必须的，所有类型的MQTT协议中，都必须包含固定头。
2. 可变头(Variable header)。存在于部分MQTT数据包中，数据包类型决定了可变头是否存在及其具体内容。可变头部不是可选的意思，而是指这部分在有些协议类型中存在，在有些协议中不存在。
3. 消息体（Payload)。存在于部分MQTT数据包中，表示客户端收到的具体内容。与可变头一样,在有些协议类型中有消息内容，有些协议类型中没有消息内容。

QOS等级划分 : 0,1,2

0 : 发一次,可能会丢

1: 至少发一次,不会丢,可能会重复

2: 不一定只发一次,不会丢,不会重复

主题:(类似于url,用于发/收数据)

'   +   '单层通配符

'  #  '多层通配符

$SYS开头为系统主题,用于获取服务器信息

消息分类:

1.保留消息,用于服务器帮离线的订阅者缓存消息

2.消息过期时间,用于缓存消息的过期

3.遗嘱消息,用于发布者下线时发布消息

4.延迟发布,用于定时发布消息 $delayed/60/topic/a : 60秒后发布给topic/a主题

订阅选项

1.QoS 

​	服务器QoS小:服务器告诉订阅者最大的QoS,让它自己去评估要不要

​	订阅要的QoS < 消息发布的QoS : 降级处理

2.No Local

​	举例:服务器A、B,当消息从A转发给B时可以让这条消息不要再发给A

3.Retain As Published

​	举例: 当A给B发保留信息时可以保留住这条消息的保留信息属性,从而让B也保留这条消息

4.Reatin Handling

​	当有新订阅时发不发保留消息

共享订阅:

​	多个消费者

带组的共享订阅

```
$share/组名/主题
```

不带组的共享订阅,全部共享

```
$queue/组名/主题
```

