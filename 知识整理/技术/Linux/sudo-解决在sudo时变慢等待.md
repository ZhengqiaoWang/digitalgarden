---
tags:
  - 知识整理/Linux/shell
---

# 1 概述

在我升级使用 Deepin20.7 的时候突然发现使用 sudo 命令总是需要等待个十秒钟，特别影响使用体验，于是经过查询发现使用 sudo 命令时，会首先通过网络查找 sudoer 名单，然后才是本地。10 秒钟恰恰是 DNS 超时时间。解决方法也很简单，只需要通过修改 hosts 将域名直接指向 localhost 即可。

# 2 解决步骤

在控制台输入

```shell
hostname
```

输出主机名。然后使用

```shell
sudo vi /etc/hosts
```

在 hosts 文件中添加一行(注意使用`Tab`实现分割)：

```text
127.0.0.1 {YOUR HOSTNAME} {YOUR HOSTNAME}.localdomain
```

例如我的`hostname`是`zhengqiaowang`，那么就可以在文件末尾追加

```text
127.0.0.1 zhengqiaowang zhengqiaowang.localdomain
```

保存后即可解决。
