查看已有的shell

```shell
cat /etc/shells
```

查看shell

```shell
echo $SHELL //系统默认的shell
echo $SHELL //目前的正在使用的shell
```

.sh文件

```sh
#!/bin/bash
语句
```

加执行权限

```sh
chmod a+x hello.sh
```

临时变量

```
local var
```

语法

```sh
#输入给name
read name
#引用参数
$1
$2
#特殊参数
$#    	参数的个数
$?		上一个命令的状态码,0表示正常,非0表示错误
$*		参数整体
$@		位置参数
$$		当前shell的pid
$!		最后一个后台命令的进程id
$0		当前脚本的名称
$RANDOM	0-32767随机数
```

定义环境变量,当前shell有效

export name=AAA

source 文件名 重新加载文件

判断

```sh
if [[]]//加强
if []//基本
if (())//支持数学运算
=========================
if [[ 条件左右要有空格$var1 -eq $var2 ]]; then
	echo "true"
elif [[   ]]; then
	echo "elif"
else
	echo "flase"
fi
```

循环

```sh
while [[ 条件 ]]
do
循环体
done
```

把命令的输出给一个变量

```bash
a=$(ls)
# 把命令输出作为字符串,使用反引号: `命令`
echo `ls`
```

运算类

```bash
# > gt, < lt, >= gq, <= lq, == eq, != ne
# 进行数学运算 $((表达式)), 自增运算 let 变量名++
```

不同的语句写在同一行的写法,用分号加空格隔开语句

```bash
# 输出 1~10
while [[ a -lt 10 ]]; do echo $a; let a++; done
```

字符串

```shell
# ''表示字符串原样输出
# 截取字符串
var1="0123456789ABCD"
${var1:0:2} #01
${var1:3} #3456789ABCD
${var1:0-4:2} #AB
${var1:0-3:2} #BC
${var1:0-5} #9ABCD
${var1#*9} #ABCD
```

数组

```shell
# 定义
name=(var1 var2)
name=([index1]=var1 [index2]=var2)
# 使用
${name[index]}
${name[@]} ${name[*]}
# 取长度
${#name[@]} 数组个数
${#name[index]} 数组元素的字符长度
# 数组拼接
array_new=(${arr1[@] ${arr2[@]}})
# 删除
unset name[index]
unset name
```

sed -> split replace

grep -> 过滤

awk -> 计算

wc -> 统计

shift 减少一个参数
