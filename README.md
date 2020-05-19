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