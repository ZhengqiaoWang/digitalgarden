---
tags:
  - 知识整理/Linux/shell
aliases:
  - man命令
---

# 1 使用

> 详见：[man 命令 – 查看帮助信息 – Linux 命令大全(手册) (linuxcool.com)](https://www.linuxcool.com/man)

当我们需要离线查看某个命令的用法时，除了命令自带的`--help`之外

```shell
{command} -h
{command} --help
```

还可以使用`man`命令查看该命令的详细文档。举一个有趣的例子：

```shell
man man
```

我们可以查看`man`命令的使用文档：

```text
MAN(1)                                                                                      Manual pager utils                                                                                     MAN(1)

NAME
       man - an interface to the system reference manuals

SYNOPSIS
       man [man options] [[section] page ...] ...
       man -k [apropos options] regexp ...
       man -K [man options] [section] term ...
       man -f [whatis options] page ...
       man -l [man options] file ...
       man -w|-W [man options] page ...

DESCRIPTION
       man  is the system's manual pager.  Each page argument given to man is normally the name of a program, utility or function.  The manual page associated with each of these arguments is then found
       and displayed.  A section, if provided, will direct man to look only in that section of the manual.  The default action is to search in all of the available sections following a pre-defined  or‐
       der (see DEFAULTS), and to show only the first page found, even if page exists in several sections.

......
```

# 2 原理

> man 命令查找和配置原理：[linux 的 man 命令深入分析\_man.config-CSDN 博客](https://blog.csdn.net/macky0668/article/details/6839564)
> 如何写一个 man 手册：[自己编写 linux 的 man 手册\_man 手册的 gz 文件-CSDN 博客](https://blog.csdn.net/youyudexiaowangzi/article/details/97137413)

`man`的命令手册是基于`groff`标准撰写的，撰写后通过`gz`压缩放到特定目录下（如`/usr/share/man/man1/`，可以通过`manpath`命令查到），`man`命令会自动查找并解压读取文件：

```shell
# 如解压cp命令
gunzip -c /usr/share/man/man1/cp.1.gz
```

下面我们基于`groff`宏写一个`jogerdemo`命令的帮助文档：

```groff
Macro         Description
.B            Bold
.BI           Bold, then italics (alternate)
.BR           Bold, then roman (alternating)
.DT           Set default tabs
.HP           Begin a hanging indent
.I            Italics
.IB           Italics, then bold (alternating)
.IP           Begin hanging tag. For options. Long tags use .TP.
.IR           Italics, then roman (alternating)
.LP           Begin paragraph
.PD           Set distance between paragraphs
.PP           Begin paragraph
.RB           Roman, then bold (alternating)
.RE           End relative indent (after .RS)
.RI           Roman, then italics (alternating)
.RS           Begin relative indent (use .RE to end indent)
.SB           Small text, then bold (alternating)
.SM           Small text. Used to show words in all caps.
.SH           Section head
.SS           Subheading within a .SH heading.
.TH           Title heading. Used once at the beginning of the man page.
.TP           Begin a hanging tag. Begins text on next line, not same line as tag.
```

我们准备一个文档：`jogerdemo.helptext`

```test
.TH JogerDemo "Haha" "ADemo" "zhengqiaoWang" "hello"
.SH section
.TP
这里是一个简单的介绍
.TP
.test \.test
测试模式
```

执行命令

```shell
gzip -c jogerdemo.helptext > jogerdemo.1
sudo cp jogerdemo.1 /usr/share/man/man1/
man jogerdemo
```

即可查看到我们写的文档

![[image-20240531112941071.png]]
