---
tags:
  - 知识整理/Linux/shell
aliases:
  - crontab命令
---

> 参考：
> -   [Linux Crontab 定时任务 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)；
> -   [19. crontab 定时任务 — Linux Tools Quick Tutorial (linuxtools-rst.readthedocs.io)](https://linuxtools-rst.readthedocs.io/zh-cn/latest/tool/crontab.html)

# 1 简介

我们可以利用`crontab`命令实现`linux`环境下的定时任务执行。

# 2 使用

`crontab`支持的命令如下：

| 参数        | 功能                           |
| ----------- | ------------------------------ |
| -e          | 编辑定时任务                   |
| -l          | 查看定时任务                   |
| -r          | 删除定时任务                   |
| -u username | 可选，指定用户，默认为当前用户 |

## 2.1 设置定时任务

用户使用`crontab -e`命令可以设置定时任务，按照`vim`的使用习惯编辑和保存。每一行是一个任务，任务格式如下：

```crontab
分钟 小时 日 月 星期 命令
```

如果特定的位置为：

- 数字，如 1，那么则在该时间点触发
- 范围，如 1-2，那么则在该时间范围触发
- 离散数组，如 1,2,3，那么则在特定时间点触发
- 星号，如\*，那么在所有时间点触发
- 除号，如/，那么以特定时间间隔触发

保存后不会及时生效，建议提前设置或重启`crontab`服务立即生效。

```shell
service cron restart
```

### 2.1.1 示例

每分钟执行一次命令

```crontab
- * * * * command
```

每周三的上午 5 点 10 分到 5 点 30 分，每间隔 3 分钟执行一次命令

```crontab
10-30/3 5 * * 3 command
```

每年 2、6 月的 1 日 0 点触发一次命令

```crontab
0 0 1 2,6 * command
```

## 2.2 查看定时任务

用户使用`crontab -l`查看当前用户下的定时任务。

## 2.3 查看日志

当`crontab`有命令执行失败时，用户在登录控制台一般会收到`mail`提示，此时可以通过查看`/var/spool/mail/root`文件来看`crontab`的执行日志了。

# 3 相关文件

- `/var/spool/cron/`目录下存放的是每个用户包括 root 的 crontab 任务，每个任务以创建者的名字命名
- `/etc/crontab`这个文件负责调度各种管理和维护任务。
- `/etc/cron.d/`这个目录用来存放任何要执行的 crontab 文件或脚本。

# 4 常见问题

## 4.1 crontab 执行时间不准确

`crontab`命令仅能保证分钟级的精度，就比如我们指定 9:10 分执行，那么它可能在 9:10:30 秒执行。

## 4.2 环境变量问题导致命令执行失败

当命令手动执行时正常但在`crontab`中执行失败时就需要注意，可能是环境变量没有导入。其原因是`crontab`执行时默认不会加载`$HOME/.bash_profile`中的环境变量，甚至一些常见的系统变量也没有正确加载。

所以要么在命令中先添加环境变量，如：

```crontab
* * * * * source /home/user/.bash_profile && command
```

要么在 crontab 中直接指定环境变量，如：

```crontab
# env
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin
HOME=/home/user

# crontab
* * * * * command
```
