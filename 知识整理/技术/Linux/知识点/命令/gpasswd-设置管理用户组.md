---
tags:
  - 知识整理/Linux/shell
  - 知识整理/Linux/权限
aliases:
  - gpasswd命令
---

> 参考：
> - [Linux gpasswd 命令 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-comm-gpasswd.html)
> - [gpasswd命令 – 设置管理用户组 – Linux命令大全(手册) (linuxcool.com)](https://www.linuxcool.com/gpasswd)

# 1 简介

`gpasswd`全称应该是`group password`，用于对用户组的管理。一般与[[groupadd-添加用户组|groupadd命令]]、[[groupmod-设置用户组ID]]、[[id-查看用户ID和组ID|id命令]]、[[usermod-设置用户ID|usermod命令]]同时使用。

# 2 使用

```shell
gpasswd [option] GROUP
```

| 参数                              | 功能          |
| ------------------------------- | ----------- |
| -a, --add 用户                    | 将用户添加到指定用户组 |
| -d, --delete 用户                 | 将用户从指定用户组移除 |
| -r, --remove-password           | 移除组密码       |
| -R, --restrict                  | 限制用户登入组     |
| -M, --members 用户,...            | 设置用户组内的用户清单 |
| -A, --administrators ADMIN,.... | 设置用户组管理员列表  |

## 2.1 示例

将指定用户加入到`docker`用户组：

```shell
gpasswd -a USER docker
```
