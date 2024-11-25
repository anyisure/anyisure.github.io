---
title: linux安装软件`
date: 2024-11-25 10:45:36
tags:
---




### 安装jdk
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
输入`java -version`校验，会显示安装的java的对应版本
```bash
[root@iZuf6acvb1aci0yxk20vjwZ jdk1.8.0_251]# java -version
java version "1.8.0_251"
Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
```

#### 卸载jdk
在全局配置文件`/etc/profile`中删除关于jdk的配置，并刷新配置`source /etc/profile`。