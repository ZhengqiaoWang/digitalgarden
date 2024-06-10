---
tags:
  - 知识整理/Linux/shell
aliases:
  - envsubst命令
---

# 1 简介

`envsubst`可以用环境变量替换文件内的环境变量占位符然后输出，配合`docker`或`kubernetes`的环境变量体系可以很方便的修改文件内的变量。

# 2 安装

使用包管理器安装`envsubst`

```shell
# yum
yum install gettext

# apt
apt-get install -y gettext
```

使用一下命令验证安装

```shell
envsubst --version
```

# 3 使用

`envsubst`的命令非常简单

我们准备一个文件（test.sh）：

```shell
echo "::" $ENV_TMP_HELLOWORLD1
echo "??" $ENV_TMP_HELLOWORLD2
```

我们可以看到是没有变量输出的。

然后我们设置一下环境变量，并使用`envsubst`处理一下它：

```shell
export ENV_TMP_HELLOWORLD1="HELLO WORLD 1"
export ENV_TMP_HELLOWORLD2="HELLO WORLD 2"

# 全部替换
envsubst < test.sh
## echo "::" HELLO WORLD 1
## echo "??" HELLO WORLD 2

# 只替换HELLO WORLD 2
envsubst '$ENV_TMP_HELLOWORLD2' < test.sh
## echo "::" $ENV_TMP_HELLOWORLD1
## echo "??" HELLO WORLD 2
```
