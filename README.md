# shell

## 变量

$0	shell脚本的名字 

$#	传递给脚本的参数个数

$$	shell脚本的进程号，脚本程序通常会用它来生成一个唯一的临时文件，如//tmp/tmpfile_$$

$*	各个参数之间用环境变量IFS中第一个字符分隔

$@	不使用IFS，即使IFS为空，参数也不会挤在一起



var="Hello Shell"

echo $var		->		Hello Shell

echo "$var" 	->		Hello Shell

echo '$var'	  ->		$var

echo \\$var	   ->		$var



## 条件

判断命令 [ 或 test

```shell
if test condition
then
	statements
elif [ condition ];then
	statements
else
	statements
fi
```



## for

```shell
for variable in values
do
	statements
done
```

```shell
#!/bin/sh

for foo in bar jo 23
do
	echo $foo
done
exit 0
```

输出：

bar

jo

23



## while

```shell
while condition; do
	statements
done
```



## until

```shell
until condition
do
	statements
done
```



## case

```shell
case variable in
	pattern [ | pattern] ...) statements;;
	pattern [ | pattern] ...) statements;;
	...
esac
```



## AND

&&



## OR

||



## 语句快

```shell
get_confirm && {
	grep -v "$cdcatnum" $tracks_file > $temp_file
	cat $temp_file > $tracks_file
	echo
	add_record_tracks
}
```



## 函数

```shell
function_name() {
	statements
}
```

```shell
#!/bin/sh

foo() {
	echo "Function foo is executing"
}
echo "starting"
foo
echo "ended"
```

1. 可以使用 local 关键字在 shell 函数中声明局部变量，局部变量将仅在函数的作用范围内有效。



## eval

```shell
foo=10
x=foo
y='$'$x
echo $x
echo '$'$x
echo $y

eval y='$'$x
echo $y
```

输出 foo

​		$foo

​		$foo

​		10

eval 命令有点像一个额外的 $



## exec

用法一：将当前 shell 替换为一个不同的程序

```shell
exec wall "Thanks for"
```

这个命令会用 wall 替换当前的 shell。脚本程序中 exec命令后面的代码都不会执行，因为执行这个脚本的 shell已经不存在了

用法二：修改当前文件描述符

```shell
exec 3< afile
```

使得文件描述符3被打开以便从文件 afile中读取数据



## exit n

0					表示成功

1～125		脚本程序可以使用的错误代码

126				文件不可执行

127				命令未找到

128及以上	出现一个信号



```shell
#!/bin/sh

if [ -f .profile ]; then
	exit 0
fi
exit 1
```

等价于

```shell
[ -f .profile ] && exit 0 || exit 1
```



## export

导出变量，一旦一个变量被 shell导出，它就可以被该 shell调用的任何脚本使用，也可以被后续依次调用的任何 shell使用。

set -a 或 set -o allexport 命令将导出它之后声明的所有变量



## set

为 shell 设置参数变量

```shell
#!/bin/sh

set $(date)
echo The month is $2

exit 0
```



## shift

shift 命令把所有参数变量左移一个位置，使 $2 变成 $1, $3 变成 $2, 原来的 $1 丢弃, $0 保持不变

shift 可指定左移次数，$*、$@ 和 $# 等其他参数也将根据参数变量的新安排做相应的变动



扫描所有的位置参数：

```shell
#!/bin/sh

while [ "$1" != "" ]; do
	echo "$1"
	shift
done

exit 0
```

