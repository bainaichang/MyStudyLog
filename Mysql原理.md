# 1. 内核态   文件系统

```
虚拟文件系统
-------------
io-scheduler , device driver ,block devcie 
磁盘 nor flash 随机存取 一次随便读 | nandflash 块页存取 一次读一块
vfs -> 1.inode提供元数据的操作方法 2.dentry 所在目录 3.superblock注册inode用的 4.file 包含open,read,close,write...
直接把内存当磁盘文件系统用

InnoDB -> OLTP

tcp server
sql解析器
o/m 映射
文件系统
```

