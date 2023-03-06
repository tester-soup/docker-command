---
title: "Docker Command"
date: 2023-03-06T14:22:40+08:00
---

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
docker images   
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
import 从tar包中的内容创建一个新的文件系统再导入为镜像
![export.png](/img/image.png )
