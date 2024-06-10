---
tags:
  - 知识整理/Linux/shell
---

# 1 背景

大家很习惯的使用[[cp-复制拷贝|cp命令]]和[[mv-移动重命名|mv命令]]配合通配符`*`来实现特定目的，如期望拷贝或移动特定目录下的所有文件：

```shell
cp foo/* bar/
mv foo/* bar/
```

如果我们有封装此类函数的需要，则需要写一个函数：

```shell
function copyTo () {
	source_path=$1
	target_path=$2

	cp $source_path $target_path
	if (( $? != 0 )); then
		echo "error!"
		exit 1
	fi
}
```

此时我们期望将目录`foo`下的所有文件

![[image-20240530172726130.png]]

拷贝到目录`bar`里，于是我们尝试调用它：

```shell
copyTo foo/* bar
```

然后我们可能会得到一堆奇奇怪怪的东西，就比如`aaa`和`bbb`文件完全一样了！

![[image-20240530172808581.png]]

这种写法是错误的。

# 2 原理

假设我们写一个函数：

```shell
function test (){
	echo $@
}
```

然后同样尝试调用：

```shell
test foo/* bar
```

我们可能会很惊讶的发现它的输出是：

```
foo/aaa foo/bbb bar
```

也就是说，shell 将路径通配符展开成多个参数，然后传递给`test`函数，然而我们的目的是将`foo/aaa`和`foo/bbb`拷贝到`bar`目录。

# 3 解决方案

我们可以利用`${!#}`获取最后一个参数，通过`for`遍历`${@:1:$# - 1}`来获取除最后一个以外的所有元素。最终得到的函数如下：

```shell
function copyTo () {
    target_path=${!#}
    for source_path in "${@:1:$# - 1}"
    do
        echo "copy" $source_path "->" $target_path
        cp $source_path $target_path
        if (( $? != 0 )); then
            echo "error!"
            exit 1
        fi
    done
}
```

我们执行一下`copyTo foo/* bar`得到

![[image-20240530173308468.png]]

完成！
