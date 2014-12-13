for循环
====
##基本格式
```sh
for 变量 in 取值列表
do
    各种操作
done
```
还有`罕见`的写法就是都写作一行里：
```shell
for 变量 in 取值列表 ; do 各种操作 ;done
```
取值列表大致可以分成枚举和迭代两类
##枚举
###普通枚举
取值列表为空格或回车符分割的字符串
```shell
for i in 'apple' 'meat' 'sleep' 'woman'
do
    echo I like $i
done
```
在终端执行该脚本for.sh。运行结果
```
jelly@X:~$ bash for.sh 
I like apple
I like meat
I like sleep
I like woman
```
###配合命令替换
命令替换即\` \`和$( )两种操作符的使用。命令替换配合for循环很常见。  
比如我系统的用户叫做**jelly**，现在我新建了一个叫做**guodong**的用户。
但是guodong用户缺少很多组权限。我想让guodong拥有jelly所在的全部组。  
那么我可以这样：
```shell
for var in `groups jelly`
do
    echo $var #打印组名
    gpasswd -a guodong $var
done
```
请用root运行该脚本，这样就完成了一个给用户guodong批量添加组的任务。
##迭代
###花括号{ }
* 数字迭代，比如{1..100}  
* 字母迭代，比如{a..z},{A..Z},{Z..A}  
* ASCII字符迭代，比如{a..A}

来计算一下1加到100的和
```shell
#!/bin/bash
ans=0
for i in {1..100}
do
    let ans+=$i
done
echo $ans
```
结果是5050.  
花括号的迭代还可以指定指定**增量**，格式如下：

    {首..尾..增量}  
来我们计算一下1到100以内的所有奇数的和：
```shell
for i in {1..100..2}
do
    echo $i
done
```
###seq
需要配合命令替换使用。seq命令的格式为：  

    seq 首数 [增量] 末数

请注意增量的位置在中间，这与前面提到的花括号不同。  
来看一个例子（*改编自《Shell Scripting Expert Recipes for Linux,Bash,and More》P114*）  
用脚本来ping一下局域网内的主机：
```shell
#!/bin/bash
PREFIX=192.168.1.
for i in `seq 100 110`
do
    echo -n "${PREFIX}$i "
    ping -c5  ${PREFIX}${i} >/dev/null 2>&1
    if [ "$?" -eq 0 ];then
        echo "OK"
    else
        echo "Failed"
    fi
done
```
当然了for循环也可以写作**for i in {100..110}**  
终端运行的结果
```
jelly@X:~$ bash ping.sh 
192.168.1.100 Failed
192.168.1.101 Failed
192.168.1.102 OK
192.168.1.103 OK
192.168.1.104 OK
192.168.1.105 OK
192.168.1.106 Failed
192.168.1.107 Failed
192.168.1.108 Failed
192.168.1.109 Failed
192.168.1.110 Failed
```

C风格for循环
========
Bash还支持C语言风格的for循环，这个很好理解，我们直接来看例子，去计算一下1到100的和。
```shell
#!/bin/bash
ans=0
for ((i=1;i<=100;i++))
do
    let ans+=$i
done
echo $ans
```
>注意!!!这里的for循环要有两层括号。


