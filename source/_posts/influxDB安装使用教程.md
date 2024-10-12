---
title: influxDB安装使用教程
categories: 数据库
tags:
  - 数据库
  - 时序数据库
  - influxDB
abbrlink: a7160305
date: 2024-10-12 16:12:39
---

# influxDB安装使用教程
## 介绍
官网：https://www.influxdata.com/


## 安装
官网安装教程地址：https://docs.influxdata.com/influxdb/v2/install/
### 通过下载安装包安装


### 通过docker安装
```.bash
docker run \
 --name influxdb2 \
 --publish 8086:8086 \
 --mount type=volume,source=influxdb2-data,target=/var/lib/influxdb2 \
 --mount type=volume,source=influxdb2-config,target=/etc/influxdb2 \
 --env DOCKER_INFLUXDB_INIT_MODE=setup \
 --env DOCKER_INFLUXDB_INIT_USERNAME=ADMIN_USERNAME \
 --env DOCKER_INFLUXDB_INIT_PASSWORD=ADMIN_PASSWORD \
 --env DOCKER_INFLUXDB_INIT_ORG=ORG_NAME \
 --env DOCKER_INFLUXDB_INIT_BUCKET=BUCKET_NAME \
 influxdb:2

```