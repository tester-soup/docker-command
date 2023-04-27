---
title: "Docker Command"
date: 2023-03-06T14:22:40+08:00
---
<!-- TOC tocDepth:2..3 chapterDepth:2..6 -->

- [安装](#安装)
  - [配置国内镜像源](#配置国内镜像源)
- [docker 服务相关命令](#docker-服务相关命令)
  - [启动](#启动)
  - [状态](#状态)
  - [停止](#停止)
  - [重启](#重启)
  - [开机启动](#开机启动)
- [docker镜像相关命令](#docker镜像相关命令)
  - [查看镜像](#查看镜像)
  - [搜索镜像](#搜索镜像)
  - [拉取镜像](#拉取镜像)
  - [删除镜像](#删除镜像)
  - [查看镜像层数](#查看镜像层数)
- [docker容器相关命令](#docker容器相关命令)
  - [查看容器](#查看容器)
  - [进入容器](#进入容器)
  - [创建并启动容器](#创建并启动容器)
  - [启动容器](#启动容器)
  - [停止容器](#停止容器)
  - [删除容器](#删除容器)
  - [查看容器信息](#查看容器信息)
  - [复制容器文件到宿主机](#复制容器文件到宿主机)
  - [导入和导出容器](#导入和导出容器)
  - [import 从tar包中的内容创建一个新的文件系统再导入为镜像](#import-从tar包中的内容创建一个新的文件系统再导入为镜像)
- [docker容器的数据卷](#docker容器的数据卷)
  - [配置数据卷](#配置数据卷)
  - [数据卷容器](#数据卷容器)
- [Docker应用部署](#docker应用部署)
  - [docker部署mysql](#docker部署mysql)
  - [docker部署tomcat](#docker部署tomcat)
  - [docker部署nginx](#docker部署nginx)
  - [docker部署redis](#docker部署redis)
- [docker-compose](#docker-compose)
  - [安装docker-compose](#安装docker-compose)
  - [查看版本](#查看版本)
  - [查看配置](#查看配置)
  - [后台启动](#后台启动)
  - [构建镜像](#构建镜像)
  - [下载镜像](#下载镜像)
  - [查看运行](#查看运行)
  - [查看进程](#查看进程)
  - [启动](#启动)
  - [停止](#停止)
- [docker commit](#docker-commit)
- [dockerfile](#dockerfile)
  - [docker 镜像原理](#docker-镜像原理)
  - [dockder build](#dockder-build)
  - [Dockerfile自定义centos](#dockerfile自定义centos)
- [制作镜像](#制作镜像)
  - [方式一（不推荐）：docker commit 容器名称 镜像名称](#方式一（不推荐）：docker-commit-容器名称-镜像名称)
  - [方式二（推荐）：dockerfile](#方式二（推荐）：dockerfile)
- [容器和虚拟机的比较](#容器和虚拟机的比较)
  - [相同](#相同)
  - [不同](#不同)

<!-- /TOC -->

## 安装
略
[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
### 配置国内镜像源
- 官方的镜像源一般比较慢，所以多半都会换成国内的源，如果你有阿里云的话，阿里云会提供一个单独地址的镜像源来使用，需要到自己的阿里云上查看对应地址，在容器镜像服务下的镜像加速器中
- 换源的步骤：新建/修改 **/etc/docker/daemon.json**文件，在其中写入内容
```shell
	{ "registry-mirrors": ["镜像源地址"] } 
```

-  文件保存之后，执行下面的语句进行加载和重启
```shell
sudo systemctl daemon-reload 
sudo systemctl restart docker 
```
-  常用的镜像源：
Docker 官方中国区：https://registry.docker-cn.com  
网易：http://hub-mirror.c.163.com  
 中国科技大学：https://docker.mirrors.ustc.edu.cn   

## docker 服务相关命令
###  启动   
```shell
systemctl start docker
```
### 状态   
  ```shell 
  systemctl status docker
  ```
### 停止   
  ```shell 
  systemctl stop docker 
  ```
### 重启   
  ```shell 
  systemctl restart docker 
  ```
### 开机启动  
  ```shell  
  systemctl enable docker
  ```

## docker镜像相关命令
### 查看镜像
```shell
# docker images   
docker images -q #查看所有镜像id
```

### 搜索镜像
```shell 
# docker search 镜像名称 
docker search redis
```

### 拉取镜像
```shell 
# docker pull 镜像名称
docker pull redis:5.0
```
### 删除镜像
```shell
# docker rmi 镜像:版本
docker rmi redis:5.0
docker rmi `docker images -q` #删除所有
```

###  查看镜像层数
```shell 
# docker history 镜像名:版本号
dokcer hoistory app:1.0
docker system df -v #可以用来扫描镜像/容器大小
```

## docker容器相关命令
### 查看容器
```shell 
docker ps #查看当前运行中的容器
docker ps -a #查看所有容器
docker ps -l #显示最近创建的容器
docker ps -n #显示最近n个创建的容器
docker ps -q #静默模式，只显示容器编号
```
### 进入容器
```shell
 docker exec 参数
 ```
例:
```shell 
[root@VM-20-3-centos ~]# docker run -id --name=test3 centos:7 /bin/bash
1d58a102dc07c099a2080b051376e500f198ff47102adac1005d5cad52118d58
[root@VM-20-3-centos ~]# docker exec -it test3 /bin/bash
[root@1d58a102dc07 /]# ls
anaconda-post.log  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@1d58a102dc07 /]# exit
exit
[root@VM-20-3-centos ~]# docker ps
CONTAINER ID   IMAGE      COMMAND       CREATED          STATUS          PORTS     NAMES
1d58a102dc07   centos:7   "/bin/bash"   50 seconds ago   Up 50 seconds             test3
```
### 创建并启动容器
 ```shell 
 docker run 参数
 ```
 例:
 ```shell 
 docker run -it --name=test centos:7 /bin/bash
 ```

- `-i` 保持运行。与-t同时使用，创建容器后进入容器中，推出容器后，容器自动关闭。     
- `-t` 为容器分配一个伪输入终端。  
- `--name` 为创建的容器命名    
- `-d` 以守护（后台）模式运行容器。创建后容器再后台运行，需要使用docker exec进入容器。退出后容器不会关闭。  
- `-it` 创建的一般称为交互式容器，-id创建的成为守护式容器。  

### 启动容器
```shell 
docker start 容器名
```
### 停止容器
```shell 
docker stop 容器id或容器名
docker kill 容器id或容器名  #强制停止容器
```
### 删除容器
```shell docker 
# docker rm 容器名
docker rm `docker ps -aq` #删除所有
docker rm  `docker ps -aqf status=exited` #删除所有状态为exited的容器
```
### 查看容器信息
```shell 
# docker inspect 容器名
docker inspect redis
```
### 复制容器文件到宿主机
```shell 
docker cp 容器id:容器内路径 宿主机路径
```
### 导入和导出容器
```shell 
docker export 容器id > 导出文件名.tar
cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号
```
### import 从tar包中的内容创建一个新的文件系统再导入为镜像
```shell
[root@VM-20-3-centos test_dir]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS       PORTS                                                    NAMES
a18026dbc78d   redis:5.0             "docker-entrypoint.s…"   9 months ago    Up 3 weeks   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp              c_redis
[root@VM-20-3-centos test_dir]# docker export a18026dbc78d  > redis.tar
[root@VM-20-3-centos test_dir]# cat redis.tar | docker import - zhouerduo/redis:5.0
sha256:59f522c3342b34e385d388a71efa12882d9890cbd61fc8b9519ac57d70fec01a
[root@VM-20-3-centos test_dir]#  docker images
REPOSITORY            TAG               IMAGE ID       CREATED          SIZE
zhouerduo/redis          5.0               59f522c3342b   41 seconds ago   106MB
```

## docker容器的数据卷
### 配置数据卷
```shell
docker run ... -v 宿主机目录（文件）：容器内目录（文件）...
docker run -it --name=test4 -v /usr/local/zeshan/docker_test/test4:/usr/local/zeshan centos:7 /bin/bash
```
注意：
1. 目录必须是绝对路径
2. 不存在目录会自动创建
3. 可以挂在多个数据卷 

### 数据卷容器

1. 创建启动数据卷容器，使用-v参数
```shell
docker run -it --name=test5 -v /test centos:7 /bin/bash'
```
2. 创建启动容器test6、test7，使用--volumes-from参数，设置数据卷
```shell
docker run -it --name=test6 --volumes-from test5 centos:7 /bin/bash
docker run -it --name=test7 --volumes-from test5 centos:7 /bin/bash
```
3. 查看容器信息
```shell
docker inspect 容器名
```
找到”Mounts“，source是实际挂载的目录。test5、test6、test7都一致。destination是自己命名的。test5、test6、test7三个容器中都有这个文件夹。

```shell
  ...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "4400eab83bbdb17bc1079d340d2714ce498aca46ab96c0638516b1ff46a8552f",
                "Source": "/var/lib/docker/volumes/4400eab83bbdb17bc1079d340d2714ce498aca46ab96c0638516b1ff46a8552f/_data",
                "Destination": "/test",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
...
```

## Docker应用部署
### docker部署mysql
#### 案例需求
docker部署mysql，并通过外部的mysql客户端操作mysqlserver
#### 实现步骤
1. 搜索mysql镜像
```shell
docker search mysql
```
2. 拉取mysql镜像
```shell
docker pull mysql:5.6
```
4. 创建容器，设置端口映射、目录映射
创建mysql文件夹，用于存放mydsql数据。进入mysql文件夹执行以下操作。
```shell
docker run -id \
-p 3307:3306 \
--name=c_mysql \
-v $PWD/conf:/ect/mydql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql:5.6
```
参数说明：   
`-p 3307:3306` 将容器的3306端口映射到宿主机的3307端口   
`-v $PWD/conf:/ect/mydql/conf.d`  将宿主机当前目录下的conf/my.cnf 挂载到容器的/etc/mysql/my.cnf   
`-v $PWD/logs:/logs`  将宿主机当前的logs目录挂载到容器的/logs   
`-v $PWD/data:/var/lib/mysql ` 将宿主机的data目录挂载到容器的 /var/lib/mysql   
`-e MYSQL_ROOT_PASSWORD=123456`  初始化root用户的密码   

5. 操作容器中的mysql
```shell
docker exec -it c_mysql /bin/bash
```

### docker部署tomcat
#### 实现步骤
1. 搜索tomcat镜像
```shell
docker search tomcat
```
3. 拉取tomcat镜像
```shell
docker pull tomcat
```
4. 创建容器，设置端口映射、目录映射
```shell
docker run -id \
-p 8088:8080 \
--name=c_tomcat \
-v /usr/local/zeshan/docker_tomcat:/usr/local/tomcat/webapps \
tomcat
```
5. 创建文件，并用外部机器访问tomcat
TODO

### docker部署nginx
#### 案例需求
docker部署nginx，并通过外部机器访问nginx
#### 实现步骤
1. 搜索nginx镜像
```shell
docker search nginx
```
2. 拉取nginx镜像
```shell
docker pull nginx
```
3. 创建容器
创建/usr/local/zeshan/docker_nginx/conf/nginx.conf文件，拷贝nginx默认的conf就行。
执行以下
```shell
docker run -id \
-p 80:80 \
--name=c_nginx \
-v /usr/local/zeshan/docker_nginx/conf/nginx.conf:/ect/nginx/nginx.conf \
-v /usr/local/zeshan/docker_nginx/logs:/var/log/nginx \
-v /usr/local/zeshan/docker_nginx/html:/usr/share/nginx/html \
nginx
```
进入/usr/local/zeshan/docker_nginx/html，
创建index.html
内容如下
```html
<h1> hello nginx docker</h1>
```

4. 使用外部机器访问nginx

### docker部署redis
#### 实现步骤
1. 搜索redis镜像
```shell
docker search redis
```
2. 拉取redis镜像
```shell
docker pull redis:5.0
```
3. 创建容器
```shell
docker run -id \
-p 6379:6379 \
--name=c_redis \
redis:5.0
```

4. 使用外部机器访问redis
```shell
redis-cli -h 服务器ip -p 6379
```
## docker-compose
### 安装docker-compose
参考
[https://www.runoob.com/docker/docker-compose.html](https://www.runoob.com/docker/docker-compose.html)
[https://dockerdocs.cn/compose/install/](https://dockerdocs.cn/compose/install/)

Linux系统
```shell
https://github.com/docker/compose/releases
curl "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
更改权限，加上执行权限
```shell
chmod +x /usr/local/bin/docker-compose
```
### 查看版本
```shell
docker-compose version
```
### 查看配置
```shell
docker-compose config 
```
### 后台启动
```shell
docker-compose up -d 
```
### 构建镜像
```shell
docker-compose build
```
### 下载镜像
```shell
docker-compose pull xxx
```
### 查看运行
```shell
docker-compose ps 
```
### 查看进程
```shell
docker-composed top 
```
### 启动
```shell
docker-compose start 
```
### 停止
```shell
docker-compose stop
```
## docker commit
```shell
docker commit 容器名 新镜像名:tag
```
一般用作从一个运行状态的容器来创建一个新的镜像。定制镜像应该使用Dockerfile来完成。默认使用commit镜像，对外不可解释，可维护性差。



## dockerfile
[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
### docker 镜像原理
- docker镜像是有特殊的文件系统叠加而成
-  最底层是bootfs，并使用宿主机的bootfs
-  第二层是root文件的rootfs，成为baseimge
-  然后往上叠加其他的镜像文件

### dockder build
`.dockerignore` 忽略文件
`docker build -f` 制定文件
`docker build -t` 添加标签
`docker build --no-cache` 不使用缓存
`docker build --build-arg `构建时变量
`arg--`指令变量
### Dockerfile自定义centos
#### 案例需求
自定义centos7镜像。
要求
1. 默认登录路径为/usr；
2. 可以使用vim
#### 实现步骤：
1. 定义父镜像：`FROM centos:7`
2. 定义作者信息：`MAINTAINER zhouerduo<zhouerduo@test.com>`
3. 执行安装vim命令：`RUN yum install -y vim`
4. 定义默认的工作路径: `WORKDIR /usr`
5. 定义容器启动执行的命令：`CMD /bin/bash`

dockerfile文件完整内容
```shell
FROM centos:7
MAINTAINER zhouerduo<zhouerduo@test.com>
RUN yum install -y vim
WORKDIR /usr/local/zhouerduo
CMD /bin/bash
```
执行
```shell
docker build -f dockerfile文件路径 -t 镜像名:镜像版本  寻址路径
```
例如
```shell
docker build -f ./centos_dockerfile -t my_centos:1 .
```
## 制作镜像
### 方式一（不推荐）：docker commit 容器名称 镜像名称
不会保存制作过程
```shell
[root@VM-20-3-centos ~]# docker commit mysql tmp
sha256:c81973709d25607a403d941d5777736d199989995b118d32caa3362d49f2b739

[root@VM-20-3-centos ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
tmp           latest    c81973709d25   2 minutes ago   519MB
```
### 方式二（推荐）：dockerfile
例[Dokerfile部署centos](#Dockerfile自定义centos)
## 容器和虚拟机的比较

### 相同
相似的资源隔离和分配优势
### 不同
 - 容器   
		1. 容器虚拟化的是操作系统。   
		2. 容器和宿主机共享内核，只能运行同一类型的操作系统。   

- 虚拟机   
		1. 虚拟机虚拟化的是硬件。   
		2. 虚拟机可以运行不同的操作系统。   
