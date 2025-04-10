---
title: TDengine安装和使用
categories: 数据库
tags:
  - 数据库
  - 时序数据库
  - 物联网
abbrlink: df35d3e7
date: 2025-04-10 16:50:03
---

## TDengine
> TDengine 是一款开源、高性能、云原生的时序数据库，且针对物联网、车联网、工业互联网、金融、IT 运维等场景进行了优化。

- TDengine 的代码，包括集群功能，都在 GNU AGPL v3.0 下开源。
- 除核心的时序数据库功能外，TDengine 还提供缓存、数据订阅、流式计算等其它功能以降低系统复杂度及研发和运维成本。

- [官网](https://www.taosdata.com/)
- [安装教程](https://docs.taosdata.com/get-started/package/)
- [github开源版源码](https://github.com/taosdata/TDengine)


### 通过docker安装
[docker安装常用软件](https://anyisure.github.io/posts/3db8b873.html)
#### 拉取镜像
如果已经安装了 Docker，首先拉取最新的 TDengine 容器镜像：
```bash
docker pull tdengine/tdengine:latest
```
或者指定版本的容器镜像：
```bash
docker pull tdengine/tdengine:3.3.3.0
```

#### 创建并运行容器
1）只需执行下面的命令：
```bash
docker run -d -p 6030:6030 -p 6041:6041 -p 6043:6043 -p 6044-6049:6044-6049 -p 6044-6045:6044-6045/udp -p 6060:6060 --restart=always --name tdengine tdengine/tdengine
```

注意：
- TDengine 3.0 服务端仅使用 6030 TCP 端口。
- 6041 为 taosAdapter 所使用提供 REST 服务端口。
- 6043 为 taosKeeper 使用端口。
- 6044-6049 TCP 端口为 taosAdapter 提供第三方应用接入所使用端口，可根据需要选择是否打开。 
- 6044 和 6045 UDP 端口为 statsd 和 collectd 格式写入接口，可根据需要选择是否打开。
- 6060 为 taosExplorer 使用端口。具体端口使用情况请参考网络端口要求。

2）如果需要将数据持久化到本机的某一个文件夹，则执行下边的命令：
```bash
docker run -d --restart=always --name tdengine \
  -v ~/data/taos/dnode/data:/var/lib/taos \
  -v ~/data/taos/dnode/log:/var/log/taos \
  -p 6030:6030 -p 6041:6041 -p 6043:6043 -p 6044-6049:6044-6049 -p 6044-6045:6044-6045/udp -p 6060:6060 \
  tdengine/tdengine
```

备注:
- /var/lib/taos: TDengine 默认数据文件目录。可通过[配置文件]修改位置。你可以修改 ~/data/taos/dnode/data 为你自己的数据目录
- /var/log/taos: TDengine 默认日志文件目录。可通过[配置文件]修改位置。你可以修改 ~/data/taos/dnode/log 为你自己的日志目录

### 查看容器
确定该容器已经启动并且在正常运行。
```bash
docker ps
```

### 进入容器
进入该容器并执行 `bash`
```bash
docker exec -it tdengine  bash
```

## TDengine 命令行界面


### 添加测试数据 

> 执行该命令`taosBenchmark -y`后，系统将迅速完成 1 亿条记录的写入过程。

执行命令`taosBenchmark -y`,系统将自动在数据库 `test` 下创建一张名为 `meters` 的超级表。
- 这张超级表将包含 `10,000` 张子表，表名从 `d0` 到 `d9999`，每张表包含 `10,000` 条记录。
- 每条记录包含 `ts（时间戳）`、`current（电流）`、`voltage（电压）`和 `phase（相位）`4 个字段。
- 时间戳范围从“2017-07-14 10:40:00 000”到“2017-07-14 10:40:09 999”。
- 每张表还带有 `location` 和 `groupId` 两个**标签**，其中，groupId 设置为 1 到 10，而 location 则设置为 California.Campbell、California.Cupertino 等城市信息。

### 进入命令行
执行 `taos`,进入命令行界面：
```bash
$ taos

taos>
```

### 测试查询

TDengine CLI（taos）输入查询命令，体验查询速度。

```bash
# 查询超级表 meters 下的记录总条数
SELECT COUNT(*) FROM test.meters;

# 查询 1 亿条记录的平均值、最大值、最小值
SELECT AVG(current), MAX(voltage), MIN(phase) FROM test.meters;

# 查询 location = "California.SanFrancisco" 的记录总条数
SELECT COUNT(*) FROM test.meters WHERE location = "California.SanFrancisco";

# 查询 groupId = 10 的所有记录的平均值、最大值、最小值
SELECT AVG(current), MAX(voltage), MIN(phase) FROM test.meters WHERE groupId = 10;

# 对表 d1001 按每 10 秒进行平均值、最大值和最小值聚合统计
SELECT _wstart, AVG(current), MAX(voltage), MIN(phase) FROM test.d1001 INTERVAL(10s);
```

## 可视化管理工具

### TDengineGUI

https://github.com/skye0207/TDengineGUI

下载地址：https://github.com/skye0207/TDengineGUI/releases/tag/v1.0.0

windows下载 `tdenginegui.Setup.1.0.0.exe`直接点击运行安装

### 