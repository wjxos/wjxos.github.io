# 虚拟化技术 -- docker

- [docker 常用操作](#docker常用操作)
    - [help](#docker--help)
    - [docker镜像的常用命令](#docker镜像的常用命令)
    - [docker容器的常用命令](#docker容器的常用命令)
- [重点--docker交互模式](#重点--docker交互模式)
    - [启动交互式容器](#启动交互式容器)    
    - [启动守护式容器](#启动守护式容器)
- [进入容器](#进入容器)

- [docker镜像](#docker镜像)    
    - [启动守护式容器](#启动守护式容器)   
- [容器数据卷](#容器数据卷) 
    - [什么是容器数据卷](#什么是容器数据卷)   
    - [添加容器数据卷](#添加容器数据卷)   
    
* **docker常用操作**
* help
```
//docker 版本
docker version
//docker info 信息
docker info 
docker [OPTIONS] COMMAND
```
* docker镜像的常用命令
```java
docker images
docker images -a 
//全部镜像的ID
docker images -aq
//查询某个镜像的名称
docker search  //docker search tomcat
docker search -s 30 tomcat // star超过30 的tomcat
docker pull  //获取镜像
docker rmi   //删除镜像
docker rmi -f //强制删除
docker rmi -f tomcat nginx  //删除多个镜像
docker rmi -f $(docker -q)  //组合命令，批量删除
docker push
```
* docker容器的常用命令
```java
docker run [OPTIONS]
docker run -it centos
//参数解释
--name="容器的名称" //为容器制定一个名字
-d //后台运行容器，并返回容器的id，也即启动守护式容器
-i //已交互模式运行容器，通常与-t同时使用
-t //为容器分配一个伪输入终端，通常与-i同时使用
-p //主机端口：docker容器端口
-P //随机分配端口
docker ps //查询容器
docker -l // 查看之前运行过的容器
docker -a
docker -qa //查看容器的ID
docker run -it --name mycentos centos //创建名为mycentos的容器
exit //退出 关闭容器
ctrl+P+Q //容器不停滞退出
docker start [容器名称能]  或者  [容器ID]
docker stop [容器名称能]  或者  [容器ID]
docker kill [容器ID] //强制停止
docker rm -f // 强制删除容器
docker rm -f $(docker ps -q) //批量删除容器
//提交正在运行的容器成为一个新的镜像
docker commit -a="作者" -m="描述" docker容器ID 包名/新镜像名:版本号

```

**重点--docker交互模式** 
* 启动交互式容器
```jshelllanguage
docker run -it --name test centos
```
* 启动守护式容器
```jshelllanguage
docker run -d --name test centos// 当容器中没有运行任何程序时，会自动停掉
//启动一个容器，守护模式，每隔2秒钟打印hello wjx
docker run -d centos /bin/sh -c "while true;do echo hello wjx; sleep 2; done"
docker logs -t 934e67b7a72f //查看容器的日志
docker logs -t -f --tail 3 934e67b7a72f //实时追加日志
```
**进入容器** 
```jshelllanguage
1. attach 容器id
2. exec 进入容器并执行一段命令返回给宿主机
```

* 文件拷贝
```jshelllanguage
docker cp 容器ID:容器内的路径 目的主机的路径
```

* **容器数据卷**
* 什么是容器数据卷

将运行的数据持久化，容器之间数据共享

* 添加容器数据卷
```jshelllanguage
docker run -it -v /宿主机绝对路径目录:/容器内目录 镜像名
docker run -it -v /myDataVolume:/dataVolumeContainer 镜像名 
```















