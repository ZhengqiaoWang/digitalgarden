---
tags:
  - 知识整理/技术/docker
  - 知识整理/Linux/权限
---

# 1 背景

默认情况下，docker 仅可以使用 root 特权用户操作执行，原因是操作 docker 时需要有`/var/run/docker.sock`的读权限。
当我们查看该文件权限是，发现对组`docker`是有该文件的`rw`权限的，因此我们可以将非特权用户添加到`docker`组即可实现非特权用户使用`docker`的目的。

# 2 方法

我们可以执行使用[[groupadd-添加用户组|groupadd]]来添加`docker`用户组：

```shell
sudo groupadd docker
```

然后使用[[gpasswd-设置管理用户组|gpasswd]]将特定用户添加到`docker`用户组：

```shell
sudo gpasswd -a {用户名} docker
```

最后重启`docker`服务即可：

```shell
sudo service docker restart
```

即可完成！
