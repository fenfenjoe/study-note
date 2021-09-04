 ## 应用容器：Docker
 
### 使用前的准备
#### 基本概念
***镜像（Image）*** ：一个镜像将配置好的操作系统、基本的运行环境打成一个包。这样部署时只要用打好包的镜像来创建一个虚拟操作系统（即Docker容器）即可。
***镜像*** 可理解为一个“安装包”，运维人员运行 ***镜像*** 后，就会生成一个 ***Docker容器*** 在本机，容器里包含了已配置好的 ***环境（例如JDK、Python）*** 、***环境变量（JAVA_HOME）*** 以及 ***应用程序*** ，无需运维人员再配置。

***镜像仓库（Docker Registry）*** ：用来存放镜像文件。

***Docker容器（Container）*** ：即一个系统运行实例（类似虚拟系统）。

***Docker engine*** ：
当人们说“Docker”时，指的就是docker engine。docker engine由以下3部分组成：
1.Docker守护进程（内核）
2.与守护进程交互的REST API（系统函数）
3.在REST API下封装的命令行接口（CLI）

#### Docker与VM（虚拟操作系统）的区别

VM一旦分配便占用固定的资源（硬盘空间、内存空间）；
Docker是一个应用，当需要创建一个Docker容器时，才分配资源，比较灵活。

#### 如何在Linux上安装Docker
使用官方脚本进行安装。

安装命令如下：
```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
也可以使用国内 daocloud 一键安装命令：
```bash
curl -sSL https://get.daocloud.io/docker | sh
```
#### 如何在Windows上安装Docker
https://www.cnblogs.com/wusha/p/8556567.html

### Docker入门
#### 启动Docker 
```
systemctl start docker
```

### Docker进阶
#### Docker仓库
#### 镜像生成脚本：Dockerfile
##### Dockerfile是干什么的？
实质就是一个制作Docker镜像的一段脚本。
##### Dockerfile语法
FROM
MAINTAINER
#### 多容器运行：Docker Compose
解决“方便在一台主机上同时运行多个容器”的需求。

#### 集群部署：Docker Swarm
解决“增加Docker服务器，对Docker服务进行扩容，并将其构建成一个Docker集群”的需求。

Raft协议：保证大多数节点存活，集群才可以使用。因此通过Docker Swarm构建集群至少需要3台主机。





#### Docker镜像的原理
***UnionFS（联合文件系统）***
是Docker镜像用到的一个分层的文件系统。
当我们在Docker pull拉取镜像时，可以看到Docker会将镜像一层一层的拉取下来。
![f2020b6e96103610135cc225351f2c77.png](en-resource://database/842:1)
举例：Mysql的镜像是如何构成的。
* 第一层：bootfs（boot file system），用于加载内核到内存（与BIOS同理）
* 第二层：rootfs（root file system），包含/dev，/proc，/bin等文件，代表不同的操作系统发行版，如CentOS，Ubuntu等

#### 如何使用docker制作镜像（Image）

#### Docker可视化工具
* portainer
* Rancher

##### portainer
***在Docker中运行portainer***
```
docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true protainer/protainer
```

### Docker常见命令
![9803283b8ef920620f5bd8defff0bd90.png](en-resource://database/862:1)

***
#### Docker应用操作

***systemctl start docker***
启动Docker守护进程

***systemctl stop docker***
停止Docker守护进程

***systemctl restart docker***
重启Docker守护进程

***docker version***
查看docker版本（亦可检查docker是否安装成功）

***docker info***
查看docker信息（根目录）

***
#### 容器操作

***docker run -d ubuntu***
用ubuntu镜像创建一个容器，后台运行（但因为没有运行任何“前台进程”，因此运行完后输入docker ps会发现，容器又停止了）

***docker run -it ubuntu /bin/bash***
用ubuntu镜像创建一个容器，并以命令行形式进入该容器

***docker run -d -P --name pytest training/webapp python app.py***
用webapp镜像创建一个容器，后台运行（-d），随机映射一个端口（-P）,并运行容器上的app.py

***run详解***
首先是run的语法如下：
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

OPTIONS ：有以下可选项（常用，可参考docker run --help）
    --name [containername] #为容器起名
    -v [目录地址] #匿名挂载
    -v [卷名]:[目录地址]  #具名挂载
    -v /[本地目录地址]:[容器内地址]  #指定路径挂载
    -d #后台运行
    -p [本地端口]:[容器端口]

IMAGE： 指需要启动的镜像名

COMMAND ：
    /bin/bash #以命令行方式进入容器
```


***exit***
退出Docker容器（不会停止容器）

***docker ps***
查看运行中的容器

***docker ps -a***
查看所有容器

***docker stop [CONTAINERID]***
停止容器（CONTAINERID可通过docker ps -a查看）

***docker start [CONTAINERID]***
启动一个已停止的容器（CONTAINERID可通过docker ps -a查看）

***docker restart [CONTAINERID]***
启动一个已停止的容器（CONTAINERID可通过docker ps -a查看）

***docker run -itd --name ubuntu-test ubuntu /bin/bash***
用ubuntu镜像创建一个容器；
命名为ubuntu-test；
加了参数d默认不会进入容器

***docker attach [CONTAINERID]***
打开已运行的终端进入容器

***docker exec -it [CONTAINERID] /bin/bash***
开启一个新的终端来进入容器

***docker inspect [CONTAINERID]***
查看容器的详细信息
State：状态（Running、Pause、Restarting...）
Image：对应的镜像ID
HostConfig.Binds：卷挂载
HostConfig.PortBinding：端口映射
Mounts：卷挂载
Configs.ENV：容器开启后的环境变量
Configs.Cmd：容器开启时运行的命令
Configs.Image：生成容器的镜像名
Configs.WorkingDir：进入容器后的默认地址
NetworkSettings.Ports：端口映射
NetworkSettings.Gateway：
NetworkSettings.IPAddress：
NetworkSettings.Networks：





***docker rm -f [CONTAINERID]***
删除容器

***docker stop $(docker ps -aq)***
删除所有容器

***docker rm $(docker ps -aq)***
删除所有容器

***
#### 镜像操作
***docker images***
列出本机里的镜像列表
* REPOSITORY：表示镜像的仓库源
* TAG：镜像的标签（下面称为“版本”）
* IMAGE ID：镜像ID
* CREATED：创建时间
* SIZE：镜像大小

***docker run -it ubuntu:14.04 /bin/bash***
使用版本为14.04的ubuntu镜像来创建容器

***docker pull [Image Name]***
手动从仓库拉取Docker镜像到本地；
如：docker pull ubuntu:14.04
当本地没有该镜像时，docker会自动从仓库拉取这个镜像。仓库地址：
https://hub.docker.com/

***docker search httpd***
查找httpd的Docker镜像

***docker rmi hello-world***
删除镜像

***docker rmi -f $(docker images -qa)***
删除所有镜像

***docker build -t [镜像标签名]***
使用当前目录下的Dockerfile创建镜像


***
#### 仓库操作
***docker login -u [username] -p [password]***
登录到仓库（dockerhub）

***
#### 网络操作

***docker network ls***
查看Docker服务器正在维护的虚拟子网
```
NETWORK ID     NAME      DRIVER    SCOPE
33a42a4a7c62   bridge    bridge    local
3857b46c9a21   host      host      local
69024ef4e0be   none      null      local
```
***
#### 集群操作（Swarm）
***docker swarm init --advertise-addr [IP]*** 
初始化一个Docker的服务集群，并赋予它一个IP地址（一般是本机的IP地址）。

***docker swarm join-token manage / docker swarm join-token worker***
获取一个令牌（若需要将其他机器加入到集群，需要先获取令牌）

***docker swarm join [JOIN-TOKEN] [IP:PORT]***
将本机加入到一个Docker集群中，该Docker集群的地址为IP：PORT

***docker node ls***
查看集群里有多少节点，及节点的详细信息（主机名、是管理节点还是工作节点等）
该命令只能在管理节点里可用
***docker swarm leave***
本机退出当前集群
***
#### 服务操作

***docker service create -p 80:8080 --name my-nginx nginx***

***docker service ls***

***docker service ps [SERVICE_NAME]***

***docker service update --replicas 3 my-nginx***
创建3个服务的副本
***docker service scale my-nginx=5***
创建5个服务的副本
***docker service rm my-nginx***
移除服务
***

#### 其他操作
***docker --help***
查看docker帮助文档

***docker run --help***
查看run命令的帮助文档（其他命令同理）

***docker cp [CONTAINERID]:路径 本机路径***
将容器内的文件拷贝到本机

***docker volumn ls***
查看所有容器的卷挂载情况
```
DRIVER    VOLUME NAME
local     0d50d9bf15664fd57e7568d9e68f6d7a89a7dcc4ebcc1ac816b0ac0859a429db
local     firstvolume

#第一个是匿名卷，第二个是具名卷；
#前两个卷都维护在/var/lib/docker/volumes/ 里；
#若这两个卷里有文件的增减，则对应容器内对应的地址也会与卷内的情况同步
```

***docker volume inspect [VOLUME NAME]***
查看某个卷的信息
```
[
    {
        "CreatedAt": "2021-08-31T23:58:58+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/firstvolume/_data",
        "Name": "firstvolume",
        "Options": null,
        "Scope": "local"
    }
]
```

### 参考
Docker官方文档 [https://docs.docker.com/](https://docs.docker.com/)
遇见狂神说：Docker入门系列视频

遇见狂神说：Docker进阶系列视频[https://www.bilibili.com/video/BV1kv411q7Qc](https://www.bilibili.com/video/BV1kv411q7Qc)
