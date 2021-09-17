# Docker教程

## Docker概述

### 1.1 Docker为什么出现？

一款产品： 开发–上线是两套不同的环境！应用环境，应用配置都是不同的！
开发与运维之间的常见问题：我在我的电脑上可以运行！版本更新，导致服务不可用！对于运维来说考验十分大。
环境配置是十分的麻烦，每一个及其都要部署环境(集群Redis、ES、Hadoop…) !费事费力。
发布一个项目( jar + (Redis MySQL JDK ES) ),项目能不能带上环境安装打包！
之前在服务器配置一个应用的环境 Redis MySQL JDK ES Hadoop 配置超麻烦了，不能够跨平台。
开发环境Windows，最后发布到Linux！开发与正式环境经常性的不一致。
传统：开发做jar包，运维来做部署！
现在：开发打包部署上线，一套流程做完！
安卓流程：java — apk —发布（应用商店）一 张三使用apk一安装即可用！
docker流程： java-jar（环境） — 打包项目帯上环境（镜像） — ( Docker仓库：商店）-----
Docker给以上的问题，提出了解决方案！

![image-20210913014040668](\img\image-20210913014040668.png)

Docker的思想就来自于集装箱！

JRE（java运行时环境） – 多个应用(比如出现端口冲突问题) – app的环境可能是交叉的，会互相干扰！
隔离：Docker核心思想！打包装箱！每个箱子是互相隔离的。

Docker通过隔离机制，可以将服务器利用到极致！

<b>本质：所有的技术都是因为出现了一些问题，我们需要去解决，才去学习！</b>

> Docker历史

2010年，几个的年轻人，就在美国成立了一家公司 dotcloud
做一些pass的云计算服务！LXC（Linux Container容器）有关的容器技术！

> Linux Container容器是一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源。

他们将自己的技术（容器化技术）命名就是 Docker
Docker刚刚延生的时候，没有引起行业的注意！dotCloud，就活不下去！
<b>开源</b>
2013年，Docker开源！
越来越多的人发现docker的优点！火了。Docker每个月都会更新一个版本！
2014年4月9日，Docker1.0发布！

> docker为什么这么火？ 因为他十分的轻巧！

在容器技术出来之前，我们都是使用虚拟机技术！
虚拟机：在window中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重！
虚拟机也属于虚拟化技术，Docker容器技术，也是一种虚拟化技术！

> vm : linux centos 原生镜像（一个电脑！） 隔离、需要开启多个虚拟机！ 几个G 几分钟
> docker: 隔离，镜像（最核心的环境 4m + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 几个M 秒级启动！

<b>聊聊Docker</b>
Docker基于Go语言开发的！开源项目！
docker官网：https://www.docker.com/
文档：https://docs.docker.com/ Docker的文档是超级详细的！
仓库：https://hub.docker.com/
<b>Docker能干嘛</b>

> 之前的虚拟机技术

![image-20201110164643704](\img\image-20201110164643704.png)

虚拟机技术缺点：

1、 资源占用十分多
2、 冗余步骤多
3、 启动很慢！

> 容器化技术

容器化技术不是模拟一个完整的操作系统

![image-20201110165104442](\img\image-20201110165104442.png)

比较Docker和虚拟机技术的不同：

传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响

> DevOps（开发、运维）

<b>应用更快速的交付和部署</b>

传统：一对帮助文档，安装程序。

Docker：打包镜像发布测试一键运行。

<b>更便捷的升级和扩缩容</b>

使用了 Docker之后，我们部署应用就和搭积木一样
项目打包为一个镜像，扩展服务器A！服务器B

<b>更简单的系统运维</b>
在容器化之后，我们的开发，测试环境都是高度一致的

<b>更高效的计算资源利用</b>
Docker是内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。

## Docker安装

Docker的基本组成

![image-2020111016324759](img\image-2020111016324759.png)

<b>镜像（image)：</b>

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像==>run==>容器（提供服务的），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

<b>容器(container)：</b>

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的.
启动，停止，删除，基本命令
目前就可以把这个容器理解为就是一个简易的 Linux系统。

<b>仓库(repository)：</b>

仓库就是存放镜像的地方！
仓库分为公有仓库和私有仓库。(很类似git)
Docker Hub是国外的。
阿里云…都有容器服务器(配置镜像加速!)

> 安装docker-环境准备

Linux要求内核3.0以上

~~~shell
➜  ~ uname -r    
4.15.0-96-generic # 要求3.0以上
➜  ~ cat /etc/os-release 
NAME="Ubuntu"
VERSION="18.04.4 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.4 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
~~~

> 安装

帮助文档：https://docs.docker.com/engine/install/

~~~shell
#1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2.需要的安装包
yum install -y yum-utils
#3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#默认是从国外的，上面这个不推荐
#推荐使用国内的
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
#更新yum软件包索引
yum makecache fast
#4.安装docker相关的 docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io
#5、启动docker
docker systemctl start docker
#6. 使用docker version查看是否服务启动成功
docker version
#7. 测试
docker run hello-world

#8. 测试结果
➜  ~ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
~~~

~~~shell
 #8.查看一下下载的镜像
➜  ~ docker images         
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
hello-world           latest              bf756fb1ae65        4 months ago        13.3kB
~~~

了解：卸载docker

~~~shell
#1. 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 是docker的默认工作路径！
~~~

**阿里云镜像加速**
1、登录阿里云找到容器服务

![image-20201110173840603](img\image-20201110173840603.png)

2、找到镜像加速器

![image-20201110173906620](img\image-20201110173906620.png)

3、配置使用

![image-20201111102539155](img\image-20201111102539155.png)

注意这个地址是一个账户一个地址。

**回顾HelloWorld流程**

![image-20201111103545914](img\image-20201111103545914.png)

docker run 流程图

![image-20201111103606187](img\image-20201111103606187.png)

**底层原理**
Docker是怎么工作的？

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机（宿主机）上。通过Socket从客户端访问！

Docker-Server接收到Docker-Client的指令，就会执行这个命令！

![image-20201111111901922](img\image-20201111111901922.png)

为什么Docker比Vm快？
1、docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
2、docker利用的是宿主机的内核,而不需要Guest OS。

> GuestOS： VM（虚拟机）里的的系统（OS）;

>  HostOS：物理机里的系统（OS）；

![image-20201111112236733](img\image-20201111112236733.png)

因此,当新建一个 容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。避免引导、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载GuestOS,返个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了这个复杂的过程,因此新建一个docker容器只需要几秒钟。

![image-20201111113240788](img\image-20201111113240788.png)

现在都os基本都支持了。

## Docker的常用命令

### 帮助命令

~~~shell
docker version    #显示docker的版本信息。
docker info       #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help #帮助命令
~~~

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

### 镜像命令

> docker images #查看所有本地主机上的镜像 可以使用docker image ls代替

> docker search 搜索镜像

> docker pull 下载镜像 docker image pull

>docker rmi 删除镜像 docker image rm

**docker images 查看所有本地的主机上的镜像**

~~~shell
➜  ~ docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
mysql                 5.7                 e73346bdf465        24 hours ago        448MB

# 解释
#REPOSITORY			# 镜像的仓库源
#TAG				# 镜像的标签
#IMAGE ID			# 镜像的id
#CREATED			# 镜像的创建时间
#SIZE				# 镜像的大小
# 可选项
Options:
  -a, --all             Show all images (default hides intermediate images) #列出所有镜像
  -q, --quiet           Only show numeric IDs # 只显示镜像的id
  
➜  ~ docker images -aq ＃显示所有镜像的id
e73346bdf465
d03312117bb0
d03312117bb0
602e111c06b6
2869fc110bf7
470671670cac
bf756fb1ae65
5acf0e8da90b
~~~

**docker search 搜索镜像**

~~~shell
➜  ~ docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   9500                [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3444                [OK]  
# --filter=STARS=3000 #搜索出来的镜像就是STARS大于3000的
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
      
➜  ~ docker search mysql --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   9500                [OK]             
mariadb             MariaDB is a community-developed fork of MyS…   3444                [OK]
~~~

**docker pull 下载镜像**

~~~shell
在这里插入代码片# 下载镜像 docker pull 镜像名[:tag]
➜  ~ docker pull tomcat:8
8: Pulling from library/tomcat #如果不写tag，默认就是latest最新版
90fe46dd8199: Already exists   #分层下载： docker image 的核心 联合文件系统
35a4f1977689: Already exists 
bbc37f14aded: Already exists 
74e27dc593d4: Already exists 
93a01fbfad7f: Already exists 
1478df405869: Pull complete 
64f0dd11682b: Pull complete 
68ff4e050d11: Pull complete 
f576086003cf: Pull complete 
3b72593ce10e: Pull complete 
Digest: sha256:0c6234e7ec9d10ab32c06423ab829b32e3183ba5bf2620ee66de866df640a027  # 签名 防伪
Status: Downloaded newer image for tomcat:8
docker.io/library/tomcat:8 #真实地址

#等价于
docker pull tomcat:8
#或者
docker pull docker.io/library/tomcat:8
~~~

**docker rmi 删除镜像**

~~~shell
➜  ~ docker rmi -f 镜像id #删除指定的镜像
➜  ~ docker rmi -f 镜像id 镜像id 镜像id 镜像id#删除指定的镜像
➜  ~ docker rmi -f $(docker images -aq) #删除全部的镜像
~~~

### 容器命令

~~~shell
docker run 镜像id 新建容器并启动
docker ps 列出所有运行的容器 docker container list
docker rm 容器id 删除指定容器
docker start 容器id #启动容器
docker restart 容器id #重启容器
docker stop 容器id #停止当前正在运行的容器
docker kill 容器id #强制停止当前容器
~~~

说明：我们有了镜像才可以创建容器，Linux，下载centos镜像来学习

> docker pull centos

**新建容器并启动**

~~~shell
docker run [可选参数] image | docker container run [可选参数] image 
#参书说明
--name="Name"		容器名字 tomcat01 tomcat02 用来区分容器
-d					后台方式运行
-it 				使用交互方式运行，进入容器查看内容
-p					指定容器的端口 -p 8080(宿主机):8080(容器)
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口(常用)
		-p 容器端口
		容器端口
-P(大写) 				随机指定端口
# 测试、启动并进入容器
➜  ~ docker run -it centos /bin/bash
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
8a29a15cefae: Already exists 
Digest: sha256:fe8d824220415eed5477b63addf40fb06c3b049404242b31982106ac204f6700
Status: Downloaded newer image for centos:latest
[root@95039813da8d /]# ls    #镜像很多基础命令都不完善
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@95039813da8d /]# exit #从容器退回主机
exit
➜  ~ ls
shell  user.txt
~~~

**列出所有运行的容器**

~~~shell
#docker ps命令 #列出当前正在运行的容器
  -a, --all             展示所有容器，不管是不是在运行
  -n, --last int        展示最近几个创建的容器
  -q, --quiet           只显示容器编号
  
  ➜  ~ docker ps   
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                    NAMES
68729e9654d4        portainer/portainer   "/portainer"             14 hours ago        Up About a minute   0.0.0.0:8088->9000/tcp   funny_curie
d506a017e951        nginx                 "nginx -g 'daemon of…"   15 hours ago        Up 15 hours         0.0.0.0:3344->80/tcp     nginx01
➜  ~ docker ps -a
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS                       PORTS                    NAMES
95039813da8d        centos                "/bin/bash"              3 minutes ago       Exited (0) 2 minutes ago                              condescending_pike
1e46a426a5ba        tomcat                "catalina.sh run"        11 minutes ago      Exited (130) 9 minutes ago                            sweet_gould
14bc9334d1b2        bf756fb1ae65          "/hello"                 3 hours ago         Exited (0) 3 hours ago                                amazing_stonebraker
f10d60f473f5        bf756fb1ae65          "/hello"                 3 hours ago         Exited (0) 3 hours ago                                dreamy_germain
68729e9654d4        portainer/portainer   "/portainer"             14 hours ago        Up About a minute            0.0.0.0:8088->9000/tcp   funny_curie
677cde5e4f1d        elasticsearch         "/docker-entrypoint.…"   15 hours ago        Exited (143) 8 minutes ago                            elasticsearch
33eb3f70b4db        tomcat                "catalina.sh run"        15 hours ago        Exited (143) 8 minutes ago                            tomcat01
d506a017e951        nginx                 "nginx -g 'daemon of…"   15 hours ago        Up 15 hours                  0.0.0.0:3344->80/tcp     nginx01
24ce2db02e45        centos                "/bin/bash"              16 hours ago        Exited (0) 15 hours ago                               hopeful_faraday
42267d1ad80b        bf756fb1ae65          "/hello"                 16 hours ago        Exited (0) 16 hours ago                               ecstatic_sutherland
➜  ~ docker ps -aq
95039813da8d
1e46a426a5ba
14bc9334d1b2
f10d60f473f5
68729e9654d4
677cde5e4f1d
33eb3f70b4db
d506a017e951
24ce2db02e45
42267d1ad80b
~~~

**退出容器**

~~~shell
exit #容器直接退出
ctrl +P +Q #容器不停止退出
~~~

**删除容器**

~~~shell
docker rm 容器id   #删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)  #删除指定的容器
docker ps -a -q|xargs docker rm  #删除所有的容器
~~~

**启动和停止容器的操作**

~~~shell
docker start 容器id	#启动容器
docker restart 容器id	#重启容器
docker stop 容器id	#停止当前正在运行的容器
docker kill 容器id	#强制停止当前容器
~~~

### 常用其他命令

**后台启动命令**

~~~shell
# 命令 docker run -d 镜像名
➜  ~ docker run -d centos
a8f922c255859622ac45ce3a535b7a0e8253329be4756ed6e32265d2dd2fac6c
➜  ~ docker ps           
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
# 问题docker ps. 发现centos 停止了
# 常见的坑，docker容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
# 以centos为例，交互模式启动之后ctrl+p+q 不停止退出，这个容器就还在启动状态
~~~

**查看日志**

~~~shell
docker logs --help
Options:
      --details        Show extra details provided to logs 
*  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
*      --tail string    Number of lines to show from the end of the logs (default "all")
*  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
➜  ~ docker run -d centos /bin/sh -c "while true;do echo 6666;sleep 1;done" #模拟日志      
#显示日志
-tf		#显示日志信息（一直更新）
--tail number #需要显示日志条数
# 例子
docker logs -t --tail n 容器id #查看n行日志
docker logs -tf 容器id #滚动更新显示日志
~~~

查看实时刷新的10条日志。

![image-20201114182528793](img\image-20201114182528793.png)

**查看容器中进程信息 ps**

> *命令 docker top 容器id*

![image-20201111185425875](img\image-20201111185425875.png)

**查看镜像的元数据**

~~~shell
# 命令
docker inspect 容器id

#测试
➜  ~ docker inspect 55321bcae33d
[
    {
        "Id": "55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066",
        "Created": "2020-05-15T05:22:05.515909071Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo 6666;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 22973,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-05-15T05:22:06.165904633Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:470671670cac686c7cf0081e0b37da2e9f4f768ddc5f6a26102ccd1c6954c1ee",
        "ResolvConfPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/hostname",
        "HostsPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/hosts",
        "LogPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066-json.log",
        "Name": "/bold_bell",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a-init/diff:/var/lib/docker/overlay2/5afcd8220c51854a847a36f52775b4ed0acb16fe6cfaec3bd2e5df59863835ba/diff",
                "MergedDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/merged",
                "UpperDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/diff",
                "WorkDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "55321bcae33d",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo 6666;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200114",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS",
                "org.opencontainers.image.created": "2020-01-14 00:00:00-08:00",
                "org.opencontainers.image.licenses": "GPL-2.0-only",
                "org.opencontainers.image.title": "CentOS Base Image",
                "org.opencontainers.image.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "63ed0c837f35c12453bae9661859f37a08541a0749afb86e881869bf6fd9031b",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/63ed0c837f35",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "b129d9a5a2cbb92722a2101244bd81a9e3d8af034e83f338c13790a1a94552a1",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.4",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:04",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "ad5ada6a106f5ba3dda9ce4bc1475a4bb593bf5f7fbead72196e66515e8ac36a",
                    "EndpointID": "b129d9a5a2cbb92722a2101244bd81a9e3d8af034e83f338c13790a1a94552a1",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.4",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:04",
                    "DriverOpts": null
                }
            }
        }
    }
]
➜  ~
~~~

**进入当前正在运行的容器**

~~~shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令 
docker exec -it 容器id bashshell

#测试
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
55321bcae33d        centos              "/bin/sh -c 'while t…"   10 minutes ago      Up 10 minutes                           bold_bell
a7215824a4db        centos              "/bin/sh -c 'while t…"   13 minutes ago      Up 13 minutes                           zen_kepler
55a31b3f8613        centos              "/bin/bash"              15 minutes ago      Up 15 minutes                           lucid_clarke
➜  ~ docker exec -it 55321bcae33d /bin/bash
[root@55321bcae33d /]# 
~~~

可以使用exit退出。

![image-20201111185850704](img\image-20201111185850704.png)

~~~shell
# 方式二
docker attach 容器id
#测试
docker attach 55321bcae33d 
正在执行当前的代码...
区别
#docker exec #进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach   # 进入容器正在执行的终端，进入的是正在运行的命令行
~~~

![image-20201111185913664](img\image-20201111185913664.png)

**从容器内拷贝到主机上**

~~~shell
docker cp 容器id:容器内路径   主机目的路径
#进入docker容器内部
➜  ~ docker exec -it  55321bcae33d /bin/bash 
[root@55321bcae33d /]# ls
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
#新建一个文件
[root@55321bcae33d /]# echo "hello" > java.java
[root@55321bcae33d /]# cat java.java 
hello
[root@55321bcae33d /]# exit
exit
➜  ~ docker cp 55321bcae33d:/java.java /    #拷贝
➜  ~ cd /              
➜  / ls  #可以看见java.java存在
bin   home            lib         mnt   run       sys  vmlinuz
boot  initrd.img      lib64       opt   sbin      tmp  vmlinuz.old
dev   initrd.img.old  lost+found  proc  srv       usr  wget-log
etc   java.java       media       root  swapfile  var  
~~~

拷贝睡一个手动过程，未来我们通过数据卷来自动化这个过程。

学习方式：将我的所有笔记敲一遍，自己记录笔记！

### 小结

![image-2020111419044583](img\image-2020111419044583.png)

~~~shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #注册或者登陆一个docker源服务器
  logout      Log out from a Docker registry # 从当前的docker registry 退出
  logs        Fetch the logs of a container # 输出当前容器的日志信息
  pause       Pause all processes within one or more containers # 暂停容器
  port        List port mappings or a specific mapping for the container # 查看映射端口对应容器的源端口
  ps          List containers # 列出容器列表
  pull        Pull an image or a repository from a registry # 拉取指定镜像或者库镜像
  push        Push an image or a repository to a registry # 推送指定镜像或者库镜像 
  rename      Rename a container # 重命名容器
  restart     Restart one or more containers # 重启动容器
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default) # 保存镜像为一个tar包
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE # 给源中镜像打标签
  top         Display the running processes of a container       # 查看容器中运行的进程信息
  unpause     Unpause all processes within one or more containers # 取消暂停容器
  update      Update configuration of one or more containers
  version     Show the Docker version information       # 查看 docker 版本号
  wait        Block until one or more containers stop, then print their exit codes #截取容器停止时的推出状态值
~~~

docker的命令是十分多的，上面我们学习的那些都是最常用的容器和镜像的命令，之后我们还会学习很多命令！

接下来就是一堆的练习

### <b>作业练习</b>

> Docker安装Nginx

~~~shell
# 1、搜索镜像 search 建议大家去docker搜索，可以看到帮助文档
# 2、下载镜像 pull
# 3、运行测试
[root@kuangshen home]# docker images
PEROSITORY       TAG         IMAGE ID         GREATED         SIZE
nginx            latest      602e111c06b6     3 weeks ago     127MB
centos           latest      470671670cac     3 months ago    237MB

# -d 后台运行
-
[root@kuangshen home]# docker run -d --name nginx01 -p 3344:80 nginx
495da2065213b8936b883c4ab4332ea4e976a17d5cc30c8a42d4076840a9677a
[root@kuangshen home]# docker ps
CONTAINER ID    IMAGE    COMMAND         		      CREATED           STATUS          PORTS         	NAMES
495da2065213	nginx	 "nginx -g 'daemon of ...'"	  3 seconds ago	    Up 3 seconds    0.0.0.0:3344->80/tcp
nginx01
[root@kuangshen home]# curl localhost:3344

# 进入容器
[root@kuangshen home]# docker exec -it nginx01 /bin/bash
root@495da2065213:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@495da2065213:/# cd /etc/nginx
root@495da2065213:/etc/nginx# ls
conf.d fastcgi_params koi-utf koi-win mime.types modules nginx.conf scgi_params uwsgi_params win-utf
root@495da2065213:/etc/nginx#
~~~

端口暴露的概念

![image-20210913025149288](img\image-20210913025149288.png)

思考问题：我们每次改动nginx配置文件，都需要进入容器内部？十分的麻烦，我要是可以在容器外部提供一个映射路径，达到在容器修改文件名，容器内部就可以自动修改？-v 数据卷



> 作业：docker来装一个tomcat

