# 概述

Shell是一个命令行解释器，它接收应用程序/用户命令，然后调用操作系统的内核。同时它还是一个功能强大的变成语言。

Linux提供的shell解释器有如下几种：

```sh
(base) [root@halory ~]# cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
/bin/tcsh
/bin/csh
```

系统默认使用的shell解析器为：

```sh
(base) [root@halory ~]# echo $SHELL
/bin/bash
```

# 脚本编写

## 入门

最最经典的hello world

```sh
#!/bin/bash
echo "Hello World"
```

执行命令的方式有三种：

```sh
sh hello.sh
bash hello.sh
# 下面这种要求当前用户有执行的权限才行，否则：Permission denied，那我们只需要修改文件权限即可：chmod +x hello.sh
./hello.sh
```



当我们将命令放在`PATH`路径里面时，可以直接输入文件来执行脚本

```sh
# 查看path路径
(base) [root@halory bin]# echo $PATH
/root/miniconda3/bin:/root/miniconda3/condabin:/usr/local/python3/bin:/usr/local/openresty/nginx/sbin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/export/server/jdk/bin:/export/server/hadoop/bin:/export/server/hadoop/sbin:/root/bin

# 或者也可以直接输入脚本文件所在的路径
(base) [root@halory bin]# /root/bin/hello.sh
Hello World
```





## 变量

### 系统预定义变量

常用的系统变量有：`HOME`、`PWD`、`SHELL`、`USER`等。

```sh
# 获取变量的语法：$变量名
# 当前用户的家目录
echo $HOME
# 当前路径
echo $PWD
# 当前的shell解析器
echo $SHELL
# 当前用户
echo $USER
```



如果想要查看所有的系统变量，可以使用`set`命令。



### 自定义变量

系统预定义变量常用的就那几个，开发中如果会用到别的就再去查。

我们自定义的变量会用得比较多，在脚本中为了确保`一致性`的问题，还是尽量多用变量。

自定义变量的语法和规则如下：

```sh
# 定义变量：变量名=变量值，等号前后不能有空格，因为会被shell隔断
key=value

# 撤销变量，用得不是很多
unset key

# 声明静态变量：readonly 变量，不能修改，也不能撤销。
readonly a
readonly a=1

# 提升为全局变量: export 变量
# 提升为全局变量后，其他的脚本文件就可以通过 $变量 的方式进行引用
export a
```





### 特殊变量

#### $n

`$0`表示文件本身，此后的`$1`、`$2`等都是执行脚本时的参数，如果参数多余十个的话，就需要写成`${xx}`的形式。

```sh
#!/bin/bash
echo $0
echo $1
echo $2

# params.sh
```

执行如下的命令，得到相应的输出：

```sh
[root@halory bin]# sh params.sh -a -b
params.sh
-a
-b
```



#### $#

`$#`用于获取输入参数的个数，常用于循环，判断参数的个数是否正确以及加强脚本的健壮性。

```sh
#!/bin/bash
echo $0
echo $1
echo $2
echo "参数个数为: $#"
```

执行下面的命令，得到相应的输出结果：

```sh
[root@halory bin]# sh params.sh -a -b
params.sh
-a
-b
参数个数为： 2
```



> 但是要注意，如果要在字符串中使用`$#`的话，字符串一定要用双引号`""`，单引号是不会解析`$#`的。



#### $\*和\$@

这两个变量均用于`获取命令行中所有参数`，但是在使用上会有些许差别，在此之前可以认为是同一个命令。

```sh
#!/bin/bash
echo "$*"
echo "$@"
```



执行上面的脚本，得到相应的输出结果：

```sh
[root@halory bin]# sh params.sh -a -b
-a -b
-a -b
```





#### $?

`$?`用于获取最后一次执行的命令返回状态，如果该变量值为`0`，则表示成功执行；如果是其他`非0`数值，则表示最后一次执行的命令出错。

```sh
# 比如获取上面脚本的执行状态
[root@halory bin]# sh params.sh -a -b
-a -b
-a -b
[root@halory bin]# echo $?
0

# 执行一次错误的命令
[root@halory bin]# cd alibaba
-bash: cd: alibaba: No such file or directory
[root@halory bin]# echo $?
1

```



## 运算符

在shell脚本中一切赋值对象都是字符串，如果想要进行计算的话，可以使用：`$((运算符))`或者`$[运算符]`，推荐使用后者。

```sh
# 默认是字符串
[root@halory bin]# echo 1+2
1+2

[root@halory bin]# echo $((1+2))
3

[root@halory bin]# echo $[1+2]
3
```



## 条件判断

条件判断有两种方式：`test condition`和`[ condition ]`。

> 注意，`[ condation ]`前后有空格。

shell常用的判断条件如下：

```sh
# 两个整数之间的比较
-eq, -ne, -lt, -le, -gt, -ge

# 判断文件的权限
-r, -w, -x

# 判断文件类型
-e # 文件是否存在
-f # 文件存在并且是一个常规文件
-d # 文件存在，并且是一个目录
```



演示示例：

```sh
# 2 大于等于 1
[root@halory bin]# test 2 -ge 1
[root@halory bin]# echo $?
0

# 2 小于等于 1
[root@halory bin]# [ 2 -le 1 ]
[root@halory bin]# echo $?
1

# 测试文件
-rwxr-xr-x 1 root root 39 Jan 10 10:14 hello.sh
-rw-r--r-- 1 root root 32 Jan 10 10:35 params.sh

# hello.sh 有写权限
[root@halory bin]# [ -w hello.sh ]
[root@halory bin]# echo $?
0

# params.sh 有执行权限
[root@halory bin]# [ -x params.sh ]
[root@halory bin]# echo $?
1


```

此外，我们还可以进行多条件判断

```sh
# &&表示上一条命令执行成功时，才执行下一条命令
# ||表示上一条命令执行失败后，才执行下一条命令

[root@halory bin]# [ -x params.sh ] && echo "可执行" || echo "不可执行"
不可执行
```



## 流程控制

### if

语法如下：

```sh
# 单分支
if [ condition ]
then
	程序
fi

# 多分支
if [ condtion ]
then
	程序
elif [ condition ]
then 
	程序
else
	程序
fi

```

实操，根据输入的数字判断成绩等级：

```sh
#!/bin/bash
if [ $1 -ge 90 ]
then
        echo "优秀"
elif [ $1 -ge 80 ]
then
        echo "良好"
else
        echo "不太ok"
fi
# score.sh
```

演示：

```sh
[root@halory bin]# sh score.sh 100
优秀
[root@halory bin]# sh score.sh 80
良好
[root@halory bin]# sh score.sh 70
不太ok
[root@halory bin]#
```



### case

语法如下：

```sh
case ${变量名} in
	"v1")
		code_1
		;;
	"v2")
		code_2
		;;
	*)
		code_default
		;;
esac
```

注意事项：

1. case行尾必须为单词`in`，每一个模式匹配必须以右括号`)`结束
2. 双分号`;;`表示命令序列结束，相当于`break`
3. 最后的`*)`表示默认模式，相当于`dafault`

> case语句似乎比较适合模式匹配，或者枚举类型



### for

语法如下：

```sh
for ((初始值;循环控制条件;变量变化))
do
	程序
done
```



实操，实现一个倒计时：

```sh
#!/bin/bash
for ((i = $1; i > 0; i--))
do
        echo $i
done
```



演示：

```sh
[root@halory bin]# sh countdown.sh 5
5
4
3
2
1
```



### fori

fori就是增强for，语法如下：

```sh
for i in 可迭代对象
do
	程序
done
```

实操，遍历参数：

```sh
#!/bin/bash
for i in $*
do
        echo $i
done

for i in $@
do
        echo $i
done
# traverse.sh
```

演示：

```sh
[root@halory bin]# sh traverse.sh a b c
a
b
c
a
b
c
```



下面就是区别`$*`和`$@`的程序，跟上面的脚本相比，只是在这两个变量外面添加了双引号`""`：

```sh
#!/bin/bash
for i in "$*"
do
        echo $i
done

for i in "$@"
do
        echo $i
done
```



执行结果：

```sh
[root@halory bin]# sh traverse.sh a b c
a b c
a
b
c
```



> 使用`$*`的话，参数会看成一个整体，而`$@`则会拆开。



### while

语法如下：

```sh
while [ condition ]
do
	程序
done
```



## 读取控制台输入

语法：

```sh
read [ options ] [ parameters ]
# 选项
	# -p 指定读取值时的提示符
	# -t 等待时间（秒），不加则表示一直等待
# 参数
	# 变量：指定读取值变量名
```

实操，提示7秒内，读取控制台的输入：

```sh
#!/bin/bash
read -t 7 -p "Enter you money in 7 seconds: " money
echo $money
```



执行效果：

```sh
[root@halory bin]# sh read_ipt.sh
Enter you money in 7 seconds: 10
10
```

