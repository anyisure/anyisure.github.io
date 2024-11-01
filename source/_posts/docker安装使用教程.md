---
title: docker安装使用教程
tags:
  - docker
  - devops
abbrlink: 37bbee8b
date: 2024-09-20 15:42:24
---

[docker官方容器仓库](https://hub.docker.com/)
# 一、安装
## 1.安装docker

> Docker 是一个开源的容器化平台，可以轻松地打包、部署和运行应用程序。以下是 Docker 的安装步骤：

1. 更新系统
 > 在安装 Docker 之前，需要确保系统已经更新到最新版本。
- 可以使用以下命令更新 Ubuntu：

```sql
sudo apt-get update
sudo apt-get upgrade
```
- centos7 ，需要3.10以上，否则就要更新
```bash
uname -r # 查看内核版本 
# 以下选一执行
yum -y update # 升级所有包同时也升级软件和系统内核；
yum -y upgrade # 只升级所有包，不升级软件和系统内核
```
注：报错iptables提示unable to initialize table ‘filter’解决办法，尝试重启服务器，即可正常启动了

注：卸载旧版本docker（如果之前安装过的话） `yum remove docker  docker-common docker-selinux docker-engine`

2. 安装 Docker
 > 执行以下命令以安装 Docker：
-  Ubuntu：
```csharp
sudo apt-get install docker.io
```
- centos7
  - 1)安装需要的软件包， yum-util 提供yum-config-manager功能，另两个是devicemapper驱动依赖
    ```bash
    yum install -y yum-utils device-mapper-persistent-data lvm2  
    ```
  - 2)设置一个yum源，下面两个都可用
    ```bash
    yum-config-manager --add-repo http://download.docker.com/linux/centos/docker-ce.repo  #（中央仓库）
    yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo #（阿里仓库）
    ```
  - 3)查看可用版本有哪些
    ```bash
    yum list docker-ce --showduplicates | sort -r
    ```
  - 4)选择一个版本并安装：yum install docker-ce-版本号
    ```bash
    yum -y install docker-ce-18.03.1.ce
    ```

3. 启动 Docker 服务
- 执行以下命令以启动 Docker 服务：

```sql
sudo systemctl start docker
```

4. 设置 Docker 自动启动
- 如果希望 Docker 在系统启动时自动启动，可以使用以下命令：

```bash
sudo systemctl enable docker
```

5. 检查 Docker 版本
- 安装完成后，可以使用以下命令检查 Docker 版本：

```css
docker --version
```
以上是在  Linux 上安装 Docker 的基本步骤。需要注意的是，在一些较旧版本的 Ubuntu 中，Docker 软件包的名称可能不同，可以通过 Docker 官方网站获取最新的安装指南。

6. 配置加速器(阿里云)
> 通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://y9iniclu.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 2.安装Docker Compose

> 在 Ubuntu Linux 上使用 apt-get 安装的` Docker` 包中包含了 Docker Engine，可以通过 docker 命令来管理容器。但是并不包含 Docker Compose 工具。因此，如果您想要使用 `docker-compose` 命令来管理多个容器应用程序，需要单独安装 Docker Compose。

以下是在 Ubuntu Linux 上安装 Docker Compose 的步骤：

1. 下载 Docker Compose
  - 可以在 Docker Compose 的 GitHub Release 页面中查找最新版本的下载链接，例如：

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
请注意：如果上述命令下载失败，可以前往 Docker Compose 的 [GitHub Release](https://github.com/docker/compose/releases)页面查找最新版本的下载链接，并将其替换为上述命令中的链接。

2. 授权 Docker Compose 可执行
  - 下载完 Docker Compose 后，需要将其授权为可执行文件：

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. 检查 Docker Compose 版本
  - 安装完成后，可以使用以下命令检查 Docker Compose 版本：

```css
docker-compose --version
```
以上是在 Linux 上安装 Docker Compose 的基本步骤，安装完成后即可使用 `docker-compose` 命令来管理多个容器应用程序。

4. Docker Compose三条运行命令
   - `docker-compose up -d`: 拉起docker-compose所包含服务，并以守护进程方式运行
   - `docker-compose restart`: 重启docker-compose所有服务
   - `docker-compose down`: 关闭docker-compose所有服务

5. docker的版本
> V1 版本的 docker-compose.yml 只被支持到 `docker-compose 1.6.x`
- V1 版本的 docker-compose.yml 文件格式主要区别就是： 
  - 没有开头的 version 声明 
  - 没有 services 声明 
  - 不支持 depends_on 
  - 不支持命名的 volumes， networks， build arguments 声明 
- V2版本仅支持单机模式
- V3版本支持单机模式也支持多机模式


## 配置加速镜像地址
### 阿里云加速（个人失效，需企业版）
参考阿里云： https://help.aliyun.com/zh/acr/user-guide/accelerate-the-pulls-of-docker-official-images

> docker版本>1.1.0

通过修改daemon配置文件`/etc/docker/daemon.json`来使用加速器
```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://[系统分配前缀].mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 其他docker镜像加速器
1)修改配置文件`/etc/docker/daemon.json`，添加加速地址
- 清华大学 Docker Hub 镜像：https://mirror.tuna.tsinghua.edu.cn/docker/
- 网易加速地址：http://hub-mirror.c.163.com
- DaoCloud Docker 镜像加速：https://reg-mirror.com

2)配置多个加速器：
```json
{
   "registry-mirrors": [
       "https://mirror.tuna.tsinghua.edu.cn/docker/",
       "http://hub-mirror.c.163.com",
       "https://reg-mirror.com"
   ]
}
```

3)重载配置信息并重启
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 二、Docker 命令

## 1.镜像操作命令

- `docker images`：列出所有本地镜像
- `docker search`：搜索远程镜像库中的镜像
- `docker pull`：从远程镜像库中下载镜像
- `docker build`：使用 Dockerfile 构建镜像
- `docker tag`：为镜像打标签
- `docker rmi`：删除一个或多个本地镜像

## 2.容器操作命令

- `docker run`：创建并运行容器
- `docker start`：启动一个或多个停止的容器
- `docker stop`：停止一个或多个正在运行的容器
- `docker restart`：重启一个或多个容器
- `docker pause`：暂停一个容器
- `docker unpause`：取消暂停一个容器
- `docker kill`：强制停止一个或多个容器
- `docker rm`：删除一个或多个容器
- `docker exec`：在运行的容器中执行命令
- `docker attach`：附加到一个正在运行的容器

## 3.数据管理命令

- `docker volume`：管理 Docker 卷
- `docker cp`：在容器和本地文件系统之间复制文件
- `docker commit`：从容器创建新的镜像

### 3.1 管理Docker 卷
#### 3.1.1 创建卷
`docker volume create mysql_data`：创建名为mysql_data的卷

### 3.2 拷贝文件
`docker cp 容器名:容器内路径    服务器上路径`  ：将文件从容器拷贝到服务器上
### 3.3 制作镜像

## 4.网络操作命令

- `docker network`：管理 Docker 网络
- `docker port`：查找容器公开的端口映射
- `docker inspect`：显示容器的详细信息

### 4.1 管理 Docker 网络
#### 4.1.1  创建网络
```bash
docker network create 网络名
```
#### 4.1.2  绑定网络
```bash
docker network connect 网络名 容器名
```

#### 4.1.3 查看网络
```bash
docker network inspect 网络名
```

## 5.容器日志命令

- `docker logs`： 管理容器运行日志
### 5.1 打印容器运行日志
- `docker logs [OPTIONS] 容器名`： 打印容器运行日志

 [OPTIONS]参数：
```
-f : 跟踪日志输出

-t : 显示时间戳

--tail :仅列出最新N条容器日志

--since：显示某个日期至今的所有日志
```


## 6.其他命令

- `docker version`：显示 Docker 版本信息
- `docker info`：显示 Docker 系统信息
- `docker login`：登录到一个 Docker 镜像库
- `docker logout`：退出 Docker 镜像库
- `docker system`：管理 Docker 系统
- `docker-compose`：使用 Compose 管理多个容器应用程序




# 三、例子
### 1. `docker images`：列出所有本地镜像
使用该命令可以查看当前系统中已经下载和创建的所有镜像。例如：

```ruby
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              3556258649b2        10 days ago         72.7MB
nginx               latest              6f715b542a2e        2 weeks ago         133MB
```
### 2.`docker run`：创建并运行容器
使用该命令可以创建并启动一个新的容器。例如，以下命令会创建一个新的 Ubuntu 容器并在其中运行 `echo` 命令输出一条消息：

```shell
$ docker run ubuntu echo "Hello, Docker!"
Hello, Docker!
```
注意，当执行 `docker run` 命令时，Docker 会首先检查本地系统中是否已经存在指定的镜像。如果本地不存在，则会从远程镜像库中自动下载。此外，如果要让容器在后台运行，可以使用 `-d `选项。

### 3.`docker build`：使用 Dockerfile 构建镜像
使用该命令可以根据 Dockerfile 构建新的镜像。例如，以下是一个简单的 Dockerfile：

```sql
FROM ubuntu:latest
RUN apt-get update && apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```
执行以下命令可以在当前目录中构建一个名为 `my-nginx` 的新镜像：

```perl
$ docker build -t my-nginx .
```
### 4. `docker run -p`：指定容器端口映射
使用该命令可以将容器内的端口映射到主机上的端口，以便从外部访问容器中的应用程序。例如，以下命令将容器中的 Nginx Web 服务器端口 `80` 映射到主机上的端口 `8080`：

```css
$ docker run -p 8080:80 nginx
```

### 5.`docker exec`：在运行的容器中执行命令
使用该命令可以在已经运行的容器中执行命令。例如，以下命令将在名为 `my-container` 的容器中执行 `ls` 命令：

```shell
$ docker exec my-container ls
```


### 6. `docker-compose`：使用 Compose 管理多个容器应用程序
使用该命令可以通过 Docker Compose 管理多个容器应用程序。例如，以下是一个简单的 Docker Compose 配置文件：

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```
执行以下命令可以启动该应用程序：

```ruby
$ docker-compose up
```

### 7. 修改容器端口（验证没生效，建议重新创建容器）
- 添加绑定端口映射
```shell
docker inspect my_rabbitmq | grep Id  # 查找容器id
cd /var/lib/docker/containers/{上一步查到的Id}  # 进入容器配置目录
vim hostconfig.json

```
- 找到绑定端口的地方修改
  hostconfig.json 配置文件中，找到 `"PortBindings":{}` 这个配置项
```json
{"PortBindings":{"15672/tcp":[{"HostIp":"","HostPort":"15672"}],"5672/tcp":[{"HostIp":"","HostPort":"5672"}],"1883/tcp":[{"HostIp":"","HostPort":"1883"}]}}
```
config.v2.json 配置文件或者 config.json 的 `"ExposedPorts": {}`
```json
{"ExposedPorts":{"15672/tcp":{},"15691/tcp":{},"15692/tcp":{},"25672/tcp":{},"4369/tcp":{},"5671/tcp":{},"5672/tcp":{},"1883/tcp":{}}}
```
- 重启 docker ：`service docker restart` 或者 `systemctl restart docker`




# 四、安装图形管理界面
## 1.portainer
> Portainer是一个轻量级的管理UI，它提供了对Docker环境的容器、镜像、网络等进行管理的功能。

```shell
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v /root/portainer/data:/data portainer/portainer
```
访问地址：  http://ip:9000

### 1.1 使用stack创建容器



## 2.Docker UI

Docker UI是一个简化的Docker管理UI，提供了对Docker容器、镜像、网络等进行可视化操作的功能。

安装命令：
```shell
docker run -d -p 9000:9000 --name docker-web --privileged --restart always docker/docker-web
```
访问地址：http://localhost:9000

## 3.Shipyard

Shipyard是一个开源的Docker管理UI，提供了多主机管理、容器、镜像、存储卷等的管理功能。

安装命令：
```shell
docker run -d -p 8080:8080 --name shipyard --restart always -v /var/run/docker.sock:/var/run/docker.sock -e HOME=/ shipyard/shipyard
```
访问地址：http://localhost:8080
