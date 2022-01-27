---
title: docker的常见命令与设置
date: 2021-12-21 19:01:28
tags:
---
# docker常用命令

## [Manual](https://docs.docker.com/engine/reference/run/)


## 镜像相关

### 创建镜像
```bash
    docker build -t <docker file name> .
```

### 查询镜像
```bash
    # 查询所有镜像
    docker images
```

```bash
    # 查询所有没有名字的镜像的ImageId
    docker images | grep none | awk '{print $3}'
```

### 删除镜像

```bash
    # 删除指定镜像
    docker rmi <image name>
```

```bash
    # 删除是none的镜像
    docker rmi $(docker images | grep none | awk '{print $3}')
```

## 容器相关
### 启动容器
```bash
    # -d 为后台 -i 为交互 -t 为产生中断 -p 为端口映射 -v 为路径映射
    # 如果包含有CMD，则覆盖DOCKER FILE中的CMD
    docker run -d -p <宿主机port>:<容器port> -v <宿主机路径>:<容器路径> [镜像名] <CMD>
```

### 查询容器
```bash 
    docker ps -a
```

### 删除容器
```bash 
    docker rm $(docker ps -a -f name=你要删的玩意 -q)
```

```bash 
    # 删除退出了的container
    docker rm $(docker ps -a -f status=exited -q)
```

### 进入容器
```bash
    # 进入容器的常规办法
    docker exec -it 容器名 bash
    # 当然也有可能有的容器没带bash
    docker attach 容器名
```

### 查询容器日志
```bash
    # 往往用于查询为啥起不起来
    docker logs 容器名
```

### 查询容器的参数
```bash
    docker inspect 容器名
```

## DockerFile
[DockerFile Manual](https://docs.docker.com/engine/reference/builder/)

常用的几个命令

### ADD && COPY
将内容从外部拷贝到容器内部

```bash
    COPY /a/b/c /a/b/c
```

### RUN
在镜像生成时执行bash命令

```bash
    RUN echo 'money money money'
```

### ENTRYPOINT
容器启动时的真正执行的入口

ENTRYPOINT has two forms:

The exec form, which is the preferred form:

ENTRYPOINT ["executable", "param1", "param2"]
The shell form:

ENTRYPOINT command param1 param2

### CMD
默认的容器启动执行命令
会被覆盖，只有最后一个会生效

The CMD instruction has three forms:

- CMD ["executable","param1","param2"] (exec form, this is the preferred form)
- CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
- CMD command param1 param2 (shell form)