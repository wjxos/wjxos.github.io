# 虚拟化技术 -- docker

* docker help
```
//docker 版本
docker version
//docker info 信息
docker info 
docker [OPTIONS] COMMAND
```
* docker 镜像的常用命令
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
* docker 容器的常用命令
```java
docker run [OPTIONS]
docker run -it centos
//参数解释
--name="容器的名称" //为容器制定一个名字
-i //已交互模式运行容器，通常与-t同时使用
-t //为容器分配一个伪输入终端，通常与-i同时使用
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
```

























