---
tags:
  - 知识整理/Linux/权限
---

# 1 前言

在使用 VSCode 的 RemoteSSH 连接远程 RedHat 服务器的时候发现总是链接失败，直接使用

```bash
ssh root@xxxx
```

这种形式可以免密登录。经过检查发现服务器默认设置了严格模式。

# 2 操作步骤

修改`/etc/ssh/sshd_config`文件，将`StrictMode`从`yes`设置为`no`：

```bash
StrictModes yes
```

改为

```bash
StrictModes no
```

保存后，重启 ssh 服务：

```bash
service sshd restart
```

# 3 原因分析

这里原因是启动了严格模式，有关 Openssl 的 StrictModes，找到相关资料说明如下：

> **StrictModes**
> OpenSSH enables the StrictModes option by default. StrictModes disable the use of public and private key authentication if the required files are not properly protected to protect public key files when security is too relaxed.
> **Troubleshoot SSH**
> When you troubleshoot collective security problems where the controller cannot start or stop a member, or you cannot view or edit configuration files by using the Admin Center, if you disable StrictModes and the problem is solved, then the ownership, group, or world access to ssh directories or files that are related to public and private key authentication are incorrect. Correct the access and reenable StrictModes.

可见严格模式下，会禁用公钥和私钥认证，但本机使用命令行为什么能访问这个存疑。
