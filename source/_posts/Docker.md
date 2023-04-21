---
title: Docker
date: 2022-03-29 19:08:00
updated: 2022-04-22 20:00:00
tags: Docker
categories: 
  - ['Docker']
cover: https://pbs.twimg.com/media/FQoy4TNXwAkV8he?format=jpg&name=small

---

# Docker

## 安装 [docker](https://docs.docker.com/engine/install/debian/)

``` sh
# 选择相应的系统版本，跟着文档安装即可
# Debian
# 更新 apt，并安装相应的包
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
# 添加 GPG 密钥
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# 设置 docker 仓库
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 更新 apt，并安装 docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

apt-cache madison docker-ce
# or 指定版本
sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
# 运行 hello world
sudo docker run hello-world
# 查看版本
docker --version
```

## 安装 [docker compose](https://docs.docker.com/compose/install/)

``` sh
# 下载 docker compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 添加权限
sudo chmod +x /usr/local/bin/docker-compose
# 查看版本
docker-compose --version
```



## 运行容器

``` sh 
docker run --name <容器名> -p 8000:3000 -d <your username>/node-web-app
```

|    参数    | 说明                                                         |
| :--------: | :----------------------------------------------------------- |
|     -d     | 守护容器，就是后台运行，退出命令窗口容器也不会停止           |
|    -it     | 交互式容器 退出命令窗口容器就停止运行了。将容器的 shell 映射到当前的 shell，你在本级窗口输入的命令，会传入容器 |
|     -p     | 宿主机端口和容器端口映射。如：`-p 8000:3000` 将容器的 3000 端口映射到本机的 8000 端口 |
|     -e     | env，环境变量。如：`-e MYSQL_ROOT_PASSWORD=123456`：初始化root用户的密码 |
|     -v     | 数据持久化映射。如：`-v volume-data:/var/lib/postgresql/data` |
|   --name   | 容器名。如：`--name hello`                                   |
| -- network | 容器网络。如：`--networ=new-network`                         |

## 容器常用操作

``` sh
docker start <container id>	# 运行容器
docker exec -it <container id> bash	# 进入容器
docker kill <container id>	# 杀死容器
docker stop <container id>	# 停止容器
docker rm <container id>	# 删除容器
docker restart <container id>	# 重启容器
docker logs <container id>	# 查看容器日志
docker ps	# 查看正在运行的容器
docker ps -a	# 查看所有容器
docker system prune	# 删除所有停止的容器
docker volume prune	# 删除所有没用到的数据卷
docker inspect <container id> # 查看容器信息
```

## 镜像常用操作

``` sh
docker build . -t <image name>:<version>	# 创建镜像，不指定 version 默认为 latest
docker images # 镜像列表, docker image ls 也可以
docker search <image name> # 查找镜像
docker pull <image name>	# 拉取镜像
docker rmi <image id | image name>	# 删除镜像
```



## Dockerfile

dockerfile 用来配置 docker 镜像，Docker 根据此文件生成 image 文件

``` dockerfile
FROM node:14	# 指定基础镜像，':' 后的数字用来指定镜像版本。这里指定了版本为 14 的 node 镜像，常用的镜像有 nginx mysql php 等
MAINTAINER <author name>	# 设置镜像作者
WORKDIR /example	# 指定工作路径为 /example
COPY . /example	# 将当前目录下的所有文件（不包含 .dockerignore 排除的路径），拷贝到 image 文件的 /example 目录
RUN pnpm install	# 在工作目录下，运行 pnpm install 命令。RUN 命令在 image 构建阶段执行，执行结果都会打包进 image 文件。RUN 命令可以有多个
ENV VERSION 14.0	# 用来设置环境变量，<key>=<value> <key2>=<value2> 或者 ENV <key> <value> 两种写法。$VERSION 使用变量
ARG NAME=HI # 设置构建环境时的环境变量。该指令仅在 Dockerfile 内有效，如在 FROM 指令之前指定，就只能用于 FROM 指令中，想在 FROM 指令后使用需重新设置。AGR 指令会被构建命令 docker build --build-arg <name>=<value> 覆盖
VOLUME /data # 定义匿名卷
USER jay # 切换到指定用户
EXPOSE 3000	#	容器对外暴露端口，允许外部连接暴露的端口
CMD ["pnpm", "start"]	# 容器启动后自动执行 pnpm start 命令。CMD 命令只能有一个。如在 docker run 后附加命令，该命令会覆盖 CMD 命令
```



