while循环
==========
和其他语言一样Bash的循环结构中也有while语句。  
##基本结构
```sh
while 条件
do
    循环体
done
```
和for语句一样，它的循环体同样是do...done结构。我们可以把while语句再折叠一下
```sh
while 条件;do
    循环体
done
```
还能进一步折叠成一行体
```sh
while 条件;do 循环体;done
```
>Bash语句大都可以写作一行，只不过可读性差

和其他编程语言不同的是，Bash中的while语句用法是比较多样的。可以根据while条件的不同，将while语句分为几类。
##while条件
###方括号[ ]
和if语句的条件相同。即操作符`[ ]`。关于`[ ]`的用法请参考之前的文章。
```sh
#!/bin/bash
n=1
while [ $n -le 10 ]
do
    echo $n
    let n++    #或者写作n=$(( $n + 1 ))
done
```
在前面的文章中，已经讲过了操作符`[ ]`和`test`语句是等价的，所以这里也可以用test语句来作条件。
###终端命令
while的条件可以是各种终端的命令。包括外部命令或bash内建（built-in）命令都可以。因为命令都是有返回值的（可以用echo $?查看），命令执行的成功与否就是while条件的真或假。

以read命令来举个例子
```sh
#!/bin/bash
while read var;do
    echo "您输入的是$var"
done
```
这个程序是个死循环，将不停地等待您的输入，并回显出来。
###死循环
除了让while条件恒成立外，编程语言都有一种简洁的死循环写法。比如C语言中典型的死循环条件是while(1)，而java中的写法是while(true)。  
而Bash中的写法则简单的多，只需要一个冒号。
```sh
#!/bin/bash
while :
do
    echo I love you forever
done
```
这是一个死循环，执行之后请用按组合键**Ctrl+C**来终止它。  
此外，还有一种死循环写法就是利用系统自带的true命令（**/bin/true**）
```sh
#!/bin/bash
while /bin/true
do
    echo I love you forever
done
```
由于我们的系统环境变量（PATH）中一般都包含了路径**/bin**，所有我们也可以简写成`while true`
####while实现菜单demo
我们或许曾经用C/C++在控制台上输出过菜单。这通常是一个do-while循环实现的，先输出菜单的每个选项，然后等待输入，
根据不同的输入执行不同的操作，然后循环再次输出菜单……。  
bash中没有do-while风格的循环，但是我们很容易用替代的方案实现该功能。用死循环+if/case条件判断语句就够了。  
```sh
#菜单demo
while :
do
    echo "1：输出成绩单"
	echo "2：输出课程表"
	echo "3：输出空闲教室"
	echo "q或Q：退出菜单"
	read input
	case input in
	    1)echo "稍等，正在为您";;
		1)echo "稍等，正在为您";;
		1)echo "稍等，正在为您";;
		1)echo "稍等，正在为您";;
	esac
done
```
##while与重定向
while语句可以联合重定向（>和<）一起使用。
###while和输入重定向<
比如从一个文件内部读入。
###while和输出重定向>
