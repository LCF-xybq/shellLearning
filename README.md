# shell

## 变量

- $0	shell脚本的名字 

- $#	传递给脚本的参数个数

- $$	shell脚本的进程号，脚本程序通常会用它来生成一个唯一的临时文件，如//tmp/tmpfile_$$

- $*	各个参数之间用环境变量IFS中第一个字符分隔

- $@	不使用IFS，即使IFS为空，参数也不会挤在一起




- var="Hello Shell"

- echo $var		->		Hello Shell

- echo "$var" 	->		Hello Shell

- echo '$var'	  ->		$var

- echo \\$var	   ->		$var




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

1. 输出：

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

1. 输出 foo

   ​		$foo

   ​		$foo

   ​		10

2. eval 命令有点像一个额外的 $



## exec

用法一：将当前 shell 替换为一个不同的程序

```shell
exec wall "Thanks for"
```

1. 这个命令会用 wall 替换当前的 shell。脚本程序中 exec命令后面的代码都不会执行，因为执行这个脚本的 shell已经不存在了

用法二：修改当前文件描述符

```shell
exec 3< afile
```

1. 使得文件描述符3被打开以便从文件 afile中读取数据



## exit n

- 0					表示成功
- 1～125		脚本程序可以使用的错误代码
- 126				文件不可执行
- 127				命令未找到
- 128及以上	出现一个信号



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

- 导出变量，一旦一个变量被 shell导出，它就可以被该 shell调用的任何脚本使用，也可以被后续依次调用的任何 shell使用。
- set -a 或 set -o allexport 命令将导出它之后声明的所有变量



## set

为 shell 设置参数变量

```shell
#!/bin/sh

set $(date)
echo The month is $2

exit 0
```



## shift

- shift 命令把所有参数变量左移一个位置，使 $2 变成 $1, $3 变成 $2, 原来的 $1 丢弃, $0 保持不变
- shift 可指定左移次数，$*、$@ 和 $# 等其他参数也将根据参数变量的新安排做相应的变动



扫描所有的位置参数：

```shell
#!/bin/sh

while [ "$1" != "" ]; do
	echo "$1"
	shift
done

exit 0
```



## trap

- trap 命令用于指定在接收到信号后将要采取的行动。
- trap 命令的一种常见用途是在脚本程序被中断时完成清理工作
- trap 有两个参数，第一个参数是接收到指定信号时将要采取的行动，第二个参数是要处理的信号名
   1. trap command signal
- 重置某个信号的处理方式到其默认值，只需将 command 设置为 -
- 如果想忽略某个信号，把 command设置为空字符串 ''



## unset

- unset 命令的作用是从环境中删除变量或函数
- 设有变量 foo，foo= 语句将变量 foo 设置为空，但 foo 仍然存在，unset foo 把变量 foo 从环境中删除



## find

- find [path] [options] [tests] [actions]
- options :
  - -mount 不搜索其他文件系统中的目录
  - -atime N 文件在 N 天之前被最后访问过
  - -mtime N 文件在 N 天之前被最后修改过
- tests :
  - !  测试取反
  - -a  两个测试都必须为真
  - -o  两个测试有一个必须为真
- 可以通过使用圆括号来强制测试和操作符的优先级，圆括号对 shell 来说有特殊的含义，所以必须使用反斜线来引用圆括号
- 如果文件名处使用匹配模式，必须在模式上使用引号
- examples：
- 搜索的文件比文件 X 要新，或者文件名以下划线开头
  - \\( -newer X -o -name "_*" \\)
- 在当前目录下搜索比文件 while2 要新的文件
  - find . -newer while2 -print
  - 若只想要普通文件，可加上 -type f
  - find . -newer while2 -type f -print
- -exec 和 -ok 命令将命令行上后续的参数作为它们参数的一部分，直到被 \; 序列终止
  - find . -newer while2 -type f -exec ls -l {} \;



## grep

- ^    一行的开头
- $    一行的结尾
- .    任意单个字符
- []    放括号内包含一个字符范围，其中任何一个字符都可以被匹配，或在字符范围前面加上 ^ 符号表示使用方向字符范围，即不匹配指定范围内的字符
- ?    匹配是可选的，最多匹配一次
- \*    必须匹配 0次或多次
- \+    必须匹配 1次或多次
- {n}     必须匹配 n 次
- {n,}    必须匹配 n 次或 n次以上
- {n,m}    匹配次数在 n 到 m 之间，包括 n 和 m



## 算术扩展

- 把准备求值的表达式括在 $((...)) 中能够更有效地完成简单的算术运算
- 两对圆括号用于算术替换，一对圆括号用于命令的执行和获取输出



## 参数扩展

- ${param:-default}		如果 param为空，则设为 default
- ${#param}                    给出 param的长度
- ${param%word}          从 param的尾部开始删除与 word匹配的最小部分，然后返回剩余部分
- ${param%%word}       从 param的尾部开始删除与 word匹配的最长部分，然后返回剩余部分
- ${param#word}           从 param的头部开始删除与 word匹配的最小部分，然后返回剩余部分
- ${param##word}         从 param的头部开始删除与 word匹配的最长部分，然后返回剩余部分