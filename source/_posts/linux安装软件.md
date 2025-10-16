---
title: linux安装软件`
abbrlink: bc19088c
date: 2024-11-25 10:45:36
categories: 运维
tags:
  - devops
  - linux
---

#

## 常用命令

### 查看文件或目录大小

```bash
# 查看磁盘空间
df -hl
# 查看当前目录总共占的容量。而不单独列出各子项占用的容量
du -sh
# 查看当前目录及以下各个文件和文件夹大小
du -lh --max-depth=1
```

具体命令:

- `df -h` 命令查看磁盘空间
- `du -ah --max-depth=1` / 查看根目录下各个文件占用情况
- `max-depth` 表示目录的深度。
- 查看某个目录` du -bsh` 命令看一下常用的 usr 目录大小
  - `du -bsh /usr` #可以看到 uer 目录占用了 8.6G

进入 usr 目录用 `find 命令`找到大于 100M 文件 `find . -size +100M`，这里的“M”必须是大写哦！

du 命令参数:

```text
-a或-all 显示目录占用的磁盘空间大小，还要显示其下目录和文件占用磁盘空间的大小。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或–total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-k或–kilobytes 以KB(1024bytes)为单位输出。
-m或–megabytes 以MB为单位输出。
-s或–summarize 仅显示总计，只列出最后加总的值。
-h或–human-readable 以K，M，G为单位，提高信息的可读性。
-x或–one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-L<符号链接>或–dereference<符号链接> 显示选项中所指定符号链接的源文件大小。
-S或–separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
-X<文件>或–exclude-from=<文件> 在<文件>指定目录或文件。
–exclude=<目录或文件> 略过指定的目录或文件。
-D或–dereference-args 显示指定符号链接的源文件大小。
-H或–si 与-h参数相同，但是K，M，G是以1000为换算单位。
-l或–count-links 重复计算硬件链接的文件。
```

`du -sh` : 查看当前目录总共占的容量。而不单独列出各子项占用的容量

`du -lh --max-depth=1` : 查看当前目录下一级子文件和子目录占用的磁盘容量。

## 常用软件

### 安装 jdk

#### 解压

将安装包放到安装的文件夹`/usr/local/src/`，然后用下面的命令解压

```bash
tar -zxvf jdk-8u251-linux-x64.tar.gz
```

#### 修改`/etc/profile`全局配置

输入命令进入编辑：`vim /etc/profile`
在文件最下面添加一下三行：

```bash
export JAVA_HOME=/usr/local/src/jdk1.8.0_251
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
```

使配置生效：`source /etc/profile`

#### 验证是否安装成功

输入`java -version`校验，会显示安装的 java 的对应版本

```bash
[root@iZuf6acvb1aci0yxk20vjwZ jdk1.8.0_251]# java -version
java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

#### 卸载 jdk

在全局配置文件`/etc/profile`中删除关于 jdk 的配置，并刷新配置`source /etc/profile`。
