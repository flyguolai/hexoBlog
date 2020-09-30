---
title: 使用docker搭建jenkins
date: 2020-09-30 17:20:34
tags:
---

## [外部依赖教程](https://www.jianshu.com/p/12c9a9654f83)

## docker出现容器重复CONFICT
```bash
docker ps -a 查看是否有停用的`docker`

docker stop `[CONTAINER_ID]`查看`id`

docker rm `[CONTAINER_ID]`
```
## 一不小心把我admin账号的权限给干掉了

修改JENKINS_HOME下的`config.xml`

将useSecurity节点修改为 `<useSecurity>false</useSecurity>`但是在docker环境下没有vi咋办呢
```bash
sudo docker cp xxxx:/etc/mysql/my.cnf /home/tom/

sudo docker cp  /home/tom/my.cnf  xxxx:/etc/mysql/
```
## 权限

`rolebase-plugin` 先配置角色，再配置角色组


## 数据持久化

`docker`里的`jenkins`配置要通过`volumn`挂载到外部，给予外部使用

启动项修改为

`-v jenkins_data:/var/jenkins_home`

完整的示例
```bash
docker run -d --name jenkins --privileged=true -p 8081:8080 -p 8090:8090 -v jenkins_data:/var/jenkins_home jenkins/jenkins:lts
```
```bash
--name jenkins  这个docker的别名叫jenkins
-p 8081:8080    8081是外部端口号，8080是内部端口号
-v jenkins_data:/var/jenkins_home   volume挂载处，volume在/var/lib/docker/volumes
```

