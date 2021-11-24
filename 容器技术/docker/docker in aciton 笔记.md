### 文章目录

- [Part 1](https://blog.csdn.net/cblstc/article/details/97660212#Part_1_1)
- - [章节一 欢迎来到docker](https://blog.csdn.net/cblstc/article/details/97660212#_docker_2)
  - [章节二 在容器中跑软件](https://blog.csdn.net/cblstc/article/details/97660212#__72)
  - [章节三 简化软件安装](https://blog.csdn.net/cblstc/article/details/97660212#__305)
  - - - [识别软件](https://blog.csdn.net/cblstc/article/details/97660212#_307)
      - [寻找并安装软件](https://blog.csdn.net/cblstc/article/details/97660212#_313)
      - [安装软件并隔离](https://blog.csdn.net/cblstc/article/details/97660212#_360)
  - [章节四 持久化存储、使用volumes共享状态](https://blog.csdn.net/cblstc/article/details/97660212#_volumes_387)
  - - - [volumes初探](https://blog.csdn.net/cblstc/article/details/97660212#volumes_391)
      - [volumes的类型](https://blog.csdn.net/cblstc/article/details/97660212#volumes_427)
      - [共享数据](https://blog.csdn.net/cblstc/article/details/97660212#_445)
      - [管理卷的生命周期](https://blog.csdn.net/cblstc/article/details/97660212#_458)
      - [卷的高级模式](https://blog.csdn.net/cblstc/article/details/97660212#_466)
  - [第五章 网络暴露](https://blog.csdn.net/cblstc/article/details/97660212#__489)
  - - - [关闭容器](https://blog.csdn.net/cblstc/article/details/97660212#_491)
      - [桥接容器](https://blog.csdn.net/cblstc/article/details/97660212#_504)
      - [连接容器](https://blog.csdn.net/cblstc/article/details/97660212#_562)
      - [开放容器](https://blog.csdn.net/cblstc/article/details/97660212#_569)
      - [容器内部的依赖](https://blog.csdn.net/cblstc/article/details/97660212#_575)
  - [第六章 隔离减少风险](https://blog.csdn.net/cblstc/article/details/97660212#__578)
  - - - [资源限制](https://blog.csdn.net/cblstc/article/details/97660212#_579)
      - [共享内存](https://blog.csdn.net/cblstc/article/details/97660212#_600)
      - [理解用户](https://blog.csdn.net/cblstc/article/details/97660212#_623)
      - [能力越大，责任越大](https://blog.csdn.net/cblstc/article/details/97660212#_674)
      - [最高权力](https://blog.csdn.net/cblstc/article/details/97660212#_687)
      - [强化容器](https://blog.csdn.net/cblstc/article/details/97660212#_697)
      - [测试用例](https://blog.csdn.net/cblstc/article/details/97660212#_699)
- [Part 2](https://blog.csdn.net/cblstc/article/details/97660212#Part_2_701)
- - [打包软件](https://blog.csdn.net/cblstc/article/details/97660212#_702)
  - - - [构建docker镜像](https://blog.csdn.net/cblstc/article/details/97660212#docker_703)
      - [深入理解docker镜像和层次](https://blog.csdn.net/cblstc/article/details/97660212#docker_760)
      - [导出导入平面文件系统](https://blog.csdn.net/cblstc/article/details/97660212#_761)
      - [版本控制最佳实践](https://blog.csdn.net/cblstc/article/details/97660212#_794)
  - [自动化构建和高级镜像](https://blog.csdn.net/cblstc/article/details/97660212#_796)
  - - - [使用Dockerfile打包Git（如果以前执行过这些步骤，直接使用缓存，如果需要完全构建，使用--no-cache）](https://blog.csdn.net/cblstc/article/details/97660212#DockerfileGitnocache_797)
      - [Dockerfile初步认识](https://blog.csdn.net/cblstc/article/details/97660212#Dockerfile_818)
      - [onbuild：下游构建时触发](https://blog.csdn.net/cblstc/article/details/97660212#onbuild_925)
      - [开始脚本和多进程容器](https://blog.csdn.net/cblstc/article/details/97660212#_955)
      - [构建哈登的镜像](https://blog.csdn.net/cblstc/article/details/97660212#_956)
  - [公私软件分发](https://blog.csdn.net/cblstc/article/details/97660212#_968)
  - - - [选择一个分发方法](https://blog.csdn.net/cblstc/article/details/97660212#_969)



# Part 1

## 章节一 欢迎来到docker

安全：想安装某个软件，但又担心中病毒？别担心，有docker。
 省时：上班忙，但又担心下班没时间照顾老婆孩子？别担心，有docker。
 分发应用：制作了一个很棒的软件，但又担心用户安装起来麻烦？别担心，有docker。
 简单：系统庞大，难以管理？别担心，有docker。

**docker的权威解释（垃圾翻译）**

> Docker是一个**命令行程序**，一个**后台守护进程**，并且提供了一系列的远程服务–解决通用问题、简化安装、运行、发布和移除软件。Docker使用了UNIX技术–容器(**Containers**)。

**docker的特点（划重点，突然觉得事情变得更加有趣了）**

1. 是一个容器，
2. 和虚拟机的本质区别是：虚拟机需要依靠硬件虚拟化，而docker直接依赖主机的linux内核，没有其他添加，保证原汁原味。
3. 隔离应用。开启docker，实际上开启了docker cli和docker daemon，每个容器都作为docker daemon的子进程，每个容器里面有独有的内存空间和资源。
4. 集装箱。把容器比作集装箱。镜像（image）是docker中可装载的单元。registries和indexes用来简化镜像的分发。（不明觉厉）

**为什么使用docker？**
 想象一下，一个软件，你要考虑这个软件在哪个平台运行，需要哪些依赖软件或资源，怎么卸载，怎么保证安全？

1. 便于管理软件。如果不使用docker，软件越多，需要引用的依赖就越复杂，整个软件系统一片混乱。使用docker，我们将软件放在一个个的容器里，互不干扰。
2. 移植性。软件要在不同平台运行。（说的我很想用docker啊，这正是我想要的）
3. 安全性。保护计算机不受恶意软件的攻击，反正软件在容器里面，你病毒再怎么强大，也跑不出来。

**为什么docker这么重要？**

1. docker提供了抽象的特性。我们要安装一个软件，我们只需要告诉docker我们要安装这个软件，其他的事情docker会帮我们解决。
2. docker为各大大公司所使用和维护，比如谷歌、微软、亚马逊，你还用担心docker不好用？
3. docker之于PC，乃app store之于移动端。

**什么时候什么地点使用docker**
 很不幸，docker只能用在linux系统软件。。

**案例：Helloworld**

1. 注册docker，安装docker win客户端，貌似需要重启两次，第二次docker会为我们开启Hyper-v（坑，开启Hyper-v后不能使用vmware，也就是说，docker和vmware你只能二选一，所以我还是把docker装虚拟机吧。）
2. 打开cmd，运行

```
docker run dockerinaction/hello_world
```

> docker run的运行流程：先在本地找镜像，如果没有，那么在docker hub（docker提供的公用注册中心）上找，如果有，下载安装，创建容器运行程序。

注意：因为镜像地址在国外，所以可以使用国内的镜像资源。这里我选择阿里云的**容器镜像服务**，百度搜索即可。在镜像中心-镜像加速器中找到加速器地址。
 win版docker配置：右下角找到docker客户端-settings-daemon-basis✔

```json
{
  "registry-mirrors": ["https://dh3y1ni0.mirror.aliyuncs.com"],
  "insecure-registries": [],
  "debug": true,
  "experimental": false
}
```

**附：docker linux环境(来自官网，一切都很顺利执行下来)**

```
# 卸载旧版本
sudo apt-get remove docker docker-engine docker.io containerd runc
# 更新apt-get
sudo apt-get update
# 允许apt获取https的资源
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
# 添加docker的key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# 验证是否有key
sudo apt-key fingerprint 0EBFCD88
# 添加稳定的资源
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
# 安装docker ce
sudo apt-get install docker-ce docker-ce-cli containerd.io
# 验证
sudo docker run hello-world
```

## 章节二 在容器中跑软件

**获取docker的命令**

```
# 列出所有顶层命令
docker help
# 列出某个命令的用法：如 docker cp --help
docker 命令名 --help
```

**案例：创建一个监控网站**

1. 安装并启动nginx镜像

```
docker run --detach --name web nginx:latest
```

安装成功后生成一串唯一标识符：a60d96d03c2f23a1fa9cacea194c42f64506c69bf5ecf06a3eff3bd254e11329

detach的意思是启动一个后台程序，也叫做守护进程daemon。

1. 安装并启动mail镜像

```
# -d是--detach的缩写
docker run -d --name mailer dockerinaction/ch2_mailer
```

1. 安装并启动交互程序

```
# interactive表示启动一个交互程序，支持标准输入流stdin
# tty表示分配一个虚拟终端
# link表示和nginx联系起来
docker run --interactive --tty --link web:web --name web_test busybox  /bin/sh
```

交互程序开启了一个unix shell，跟nginx联系
 测试是否连通nginx：

```
wget -O - http://web:80/
```

ctrl+p+q：终端后台挂起

1. 安装并启动监听代理

```
docker run -it --name agent --link web:insideweb --link mailer:insidemailer dockerinaction/ch2_agent
```

代理监听的作用是监听nginx服务器，如果挂掉，那么通过mailer发送消息。

1. 测试

5.1 查看容器状态

```
docker ps
```

5.2 重启容器

```
docker restart nginx
```

刚才启动了nginx，但是由于重启了电脑，导致docker ps找不到nginx的记录，可以通过restart命令重启nginx。

5.3 查看日志

```
# --follow 或 -f
docker logs --follow web
```

由于代理会一直测试nginx的状态，所以nginx会产生日志，从而可以判断nginx是否运行。但是这样会导致日志越来越大，第四章我们会看到volumes的使用。

5.4 关闭容器

```
docker stop web
```

关闭nginx时，代理监听到变化，调用mailer发送消息，同时mailer中也存有该日志

```
docker logs mailer
```

**PID命名空间**

1. PID是linux中用来唯一标识进程的，PID命名空间是一组PID的集合

```
docker run -d --name namespaceA busybox /bin/sh -c "sleep 30000"
docker run -d --name namespaceB busybox /bin/sh -c "nc -l -p 0.0.0.0:80"

docker exec namespaceA ps
docker exec namespaceB ps

```

```
# 输出

PID   USER     TIME  COMMAND
    1 root      0:00 sleep 30000
    6 root      0:00 ps
```



1. 当然可以不使用命名空间

```
docker run --pid host busy ps
```

1. 如果一个容器已经占用一个应用程序的资源，那么另一个容器使用这个应用程序会访问不到资源。

```
docker run -d --name webConflict nginx:latest
docker logs webConflict
```

上面开启了另一个nginx，但是获取不到日志内容。原因是nginx的资源已经被web占用了。

```
docker exec webConflict nginx -g 'daemon off;'
```

执行上面命令，会提示端口占用错误。

**排除冲突**
 假设你要创建多个nginx实例，如果容器名称一样，会冲突，最简单的办法是使用id去标识，我们启动离线应用时会返回一个n位字符串，我们只需要取前12位。

```
docker exec 7cb5d2b9a7ea ps
docker stop 7cb5d2b9a7ea
```

当然，有一种情况，交互启动程序时，不会返回id，我们可以创建应用程序但不启动它，这样会产生id。

```
docker create nginx
```

创建容器ID，为什么要搞这个破玩意？因为CID文件能够位容器所共享。

```
docker create --cidfile /tmp/web.cid nginx
cat /tmp/web.cid
```

怎么找到容器ID呢？

```
docker ps --no-trunc
```

怎么查看所有容器（包括哪些docker create创建的容器）？

```
docker ps -a
```

**创建一个环境无知论系统

1. 只读文件系统
    workpress案例

```
# 开启一个只读的workpress
docker run -d --name wp --read-only wordpress:4
```

查看是否开启workpress：结果为false

```
docker inspect --format "{{.State.Running}}" wp
```

为什么没开启成功，我们看下日志。报错了，书上写的是找不到MySQL依赖，但是我这里是其他错误

```
docker logs wp
```

那么我们运行MySQL容器

```
docker run -p 3306:3306 --name wpdb -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5
```

把workpress和mysql联系起来

```
docker run -d --name wp2 --link wpdb:mysql -p 80 --read-only wordpress:4
```

执行docker logs wp2, 发现还是没启动成功报错`Fatal Error Unable to create lock file`，应该使用如下的命令

```
docker run -d --name wp3 --link wpdb:mysql -p 80 -v /run/lock/apache2/ -v /run/apache2/ --read-only wordpress:4
```

**使用环境变量**
 环境变量的重要性：为独立的容器之间共享数据

1. linux系统查看环境变量

```
env
```

1. 容器注入环境变量，如果容器已经有该环境变量，会覆盖

```
docker run --env MY_ENV_VAR="Helloworld" busybox env
```

注入环境变量有什么用呢？假设我们的wordpress需要在连接另一台服务器的mysql，那么我们可以注入`WORDPRESS_DB_HOST`指向该服务器，我们也可以很方便的设置账密，`WORDPRESS_DB_USER`和`WORDPRESS_DB_PASSWORD`，当然还可以设置数据库的名称`WORDPRESS_DB_NAME`

```
docker create --link wpdb:mysql -e WORDPRESS_DB_NAME=db_name1 -e  WORDPRESS_DB_USER=cbl -e WORDPRESS_DB_PASSWORD=root wordpress:4
```

所以最终的命令为：

```
# 运行一个mysql容器
DB_CID=$(docker run -p 3306:3306 --name wpdb -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5)
# 运行一个mailer容器
MAILER_CID=$(docker run -d dockerinaction/ch2_mailer)
# 将wordpress和mysql连接
WP_CID=$(docker create --link $DB_CID:mysql --name wp_5 -p 80 -v /run/lock/apache2/ -v /run/apache2/ -v --read-only wordpress:4)
docker start $WP_CID
# 运行一个agent，将wp和mailer连接
AGENT_CID=$(docker create --name agent_5 --link $WP_CID:insideweb --link $MAILER_CID:insidemailer dockerinaction/ch2_agent)
docker start $AGENT_CID
```

**创建持久的应用程序：就是说你的容器因为某种因素挂了，能够快速保存信息，并恢复**

1. docker重启的指数补偿策略(exponential backoff strategy)：假设第一次2s后重启，第二次可能就4s，第三次就8s，以此类推。

```
docker run -d --name test-always-restart --restart always busybox date
```

但是这个策略有个缺点：就是在重启等待阶段，容器是没有运行的，这时容器处于重启状态，于是，你执行下面命令，会报这样的错误`is restarting, wait until the container is running`

```
docker exec test-always-restart echo Just a Test
```

1. 使用监理进程(supervisor process)快速启动容器
    可以使用诸如：`init`,`systemd`,`supervisord`等监理进程。
    案例：lamp使用supervisord重启容器

```
docker run -d -p 80:80 --name lamp-test tutum/lamp
```

查看哪些进程正在使用

```
docker top lamp-test
```

杀死某个容器，验证重启。首先查看PID，我们可以发现PID=1的进程为supervisord，apache2的PID为437

```
docker exec lamp-test ps
```

杀死apache2

```
docker exec lamp-test kill 437
```

然后再查看PID，我们发现apache2又复活了，只是这次的PID为839，说明已经重启了。

```

```

**清除垃圾，你是什么垃圾**
 容器的删除

```
docker rm wb
```

如果容器正在启动、暂停、重启状态，会报错，那么我们可以强制删除

```
docker rm -f wb
```

如果希望一个容器在它退出时自动卸载，可以使用–rm命令

```
docker run --rm --name auto-exit-test busybox echo Helloworld
```

## 章节三 简化软件安装

软件安装三要素：识别软件-寻找软件-决定安装哪些文件，并且如何隔离

#### 识别软件

**仓库**
 注册主机/用户名/软件名_tag，其中tag标记着软件的版本

```
quay.io/dockerinaction/ch3_hello_registry_v2
```

#### 寻找并安装软件

Docker Hub：Docker默认的注册中心和仓库搜索引擎。
 两种方式上传自己的软件：1. 在自己的环境push到Docker Hub，非受信任。2. 创建一个Dockerfile然后让Docker Hub自己去构建，受信任。
 **搜索软件**

```
docker search alibaba
```

**寻宝小游戏**

```
docker run -it --rm dockerinaction/ch3_ex2_hunt
```

会提示你输入密码，密码在dockerinaction的所有容器中查找。

**非docker.hub的方式**

- 使用其他的注册中心

加上注册中心的host名称

```
docker pull quay.io/dockerinaction/ch3_hello_registry
```

删除

```
docker rmi quay.io/dockerinaction/ch3_hello_registry
```

- 加载镜像文件

为了演示方便，我们需要先保存一个镜像文件

```
docker pull busybox
docker save -o my-busybox.tar busybox
docker rmi busybox
```

加载镜像文件

```
docker load -i my-busybox.tar
```

- 使用dockerfile

```
git clone https://github.com/dockerinaction/ch3_dockerfile.git
docker build -t dia_ch3/dockerfile ch3_dockerfile
# 删除
docker rmi dia_ch3/dockerfile
rm -rf ch3_dockerfile
```

#### 安装软件并隔离

**镜像层次(layer)**
 镜像层次就是镜像依赖的其他镜像，如果已经下载，那么就跳过。

```
docker pull dockerinaction/ch3_myapp
docker pull dockerinaction/ch3_myotherapp
```

查看当前有哪些镜像

```
docker images
```

移除镜像

```
docker rmi dockerinaction/ch3_myapp dockerinaction/ch3_myotherapp java:6
```

**容器文件系统抽象和隔离**
 这块涉及到linux的ufs/mnt/chroot，晦涩难懂。反正就是很多好处啦：共享（如果依赖已经存在，那么就不必再下载了）、定制化（比如用户想最小化安装）。
 [ufs的一些理解](https://www.terriblecode.com/blog/how-docker-images-work-union-file-systems-for-dummies/)
 layer有三个层次基础层、覆盖层、区别层。
 `基础层`
 镜像的最原始的部分，比如：投影上的内容。
 `覆盖层`
 用户与镜像交互时修改的部分。投影上的内容+标记。
 `区别层`
 覆盖层修改的部分。比如：投影上的标记

## 章节四 持久化存储、使用volumes共享状态

本章的核心：

- volumes介绍、两种volumes、volumes生命周期、volumes的数据管理和控制模式
- 主机和容器、容器和容器共享数据

#### volumes初探

大概了解下，volumes是用来跟数据打交道的。
 **NoSql数据库使用volumes**
 安装cassandra数据库并启动命令行

```
docker run -d --volume /var/lib/cassandra/data --name my-cass alpine echo Data Container
# 安装cass
docker run -d --volumes-from my-cass --name cass cassandra
# 连接cqlsh命令行
docker run -it --rm --link cass:cass cassandra cqlsh cass
```

创建keyspace并查询

```
create keyspace docker_helloworld with replication = {'class': 'SimpleStrategy', replication_factor': 1};
describe docker_helloworld;
```

删库跑路（其实只是删除数据库，不删数据）

```
quit
docker stop cass
docker rm -vf cass
```

再搞一个cass容器，测试数据是否存在，经测试已经存在

```
docker run -d --volumes-from my-cass --name cass2 cassandra
docker run -it --rm --link cass2:cass cassandra cqlsh cass
describe keyspaces
quit
docker stop cass2
docker rm -vf cass2
```

该案例结束，毁尸灭迹

```
docker rm -vf my-cass
```

#### volumes的类型

**绑定挂载卷：Bind mount volumes**
 用户可以自定义挂载点，不固定。
 小案例：运行一个http容器，挂载主机的文件，我再temp目录下放了一个home.html文件，ro表示只读，容器没有权限修改主机的文件。

```
docker run -d --name myweb -v /home/cbl/Develop/temp:/usr/local/apache2/htdocs:ro -p 7788:7788 httpd
```

**管理卷：Managed volumes**
 docker守护进程指定的路径，固定的：/var/lib/docker/vfs/dir/
 使用-v命令如果不指定目录，表示使用管理卷。

```
docker run -d -v /var/lib/cassandra/data --name my-cass-shared alpine echo Data Container
```

查看卷的位置

```
docker inspect -f "{{json .Volumes}}" my-cass-shared
```

#### 共享数据

**主机共享数据**
 前面讲到的bind mount就是这种方式，下面使用一个读写日志的案例来说明数据是如何共享的

```
# 创建一个写日志容器，会一直产生日志
docker run --name log-writer -d -v /home/cbl/Develop/temp/web-logs-example:/data dockerinaction/ch4_writer_a
# 创建一个读日志容器，读取写日志容器的日志
docker run --rm -v /home/cbl/Develop/temp/web-logs-example:/reader-data alpine head /reader-data/logA
cat /home/cbl/Develop/temp/web-logs-example/logA
```

**容器共享数据**
 一个容器引用另一个容器的volume，比如之前的cass容器引用my-cass-shared容器的volume，语法是`volumes-from`

#### 管理卷的生命周期

**管理卷的删除**
 删除容器时，使用docker rm -v命令，-v命令会删除容器依赖的卷，但是如果卷被其他容器所依赖，就不会被删除。如果删除容器时，不使用-v命令，会导致卷成为孤立的块，虽然可以用命令删除，但是很麻烦。所以建议在删除容器的时候使用-v命令。
 删除所有的在停止状态的容器

```
docker rm -v $(docker ps -aq)
```

#### 卷的高级模式

**卷容器模式**
 创建一个依赖主机卷的容器，让其他容器依赖该容器，使用–volumes-from
 **卷容器模式的进阶版：数据卷容器**
 其实就是卷容器把另一个区域的数据拷贝到卷指向的位置，其他容器就能够共享这些数据。

```
docker run --name my-shared -v /config dockerinaction/ch4_packed /bin/sh -c 'cp /packed/* /config/'
docker run --rm --volumes-from my-shared alpine ls /config
```

**多态容器模式**
 编程语言中的多态表示一种规范，多种实现。容器的多态指的是你创建一个容器的同时还可以额外增加一些命令，比如之前的echo ‘Helloworld’, /bin/sh -c 'xxx’等。
 我们使用数据卷容器来模拟开发和生产环境配置的打包

```
# 本地配置
docker run --name dev-conf -v /config dockerinaction/ch4_packed_config /bin/sh -c 'cp /development/* /config/'
# 生产配置
docker run --name prod-conf -v /config dockerinaction/ch4_packed_config /bin/sh -c 'cp /production/* /config/'
# 加载本地配置
docker run --name dev-app --volumes-from dev-conf dockerinaction/ch4_polyapp
# 加载生产配置
docker run --name prod-app --volumes-from prod-conf dockerinaction/ch4_polyapp
```

## 第五章 网络暴露

四种网络容器原型：关闭容器、桥接容器、连接容器、开启容器

#### 关闭容器

只能访问私有回环网络（127.0.0.1），不允许其他网络过来，俗称断网。
 **创建一个关闭容器**

```
docker run --rm --net none alpine ip addr
```

验证网络

```
docker run --rm --net none alpine ping -w 2 8.8.8.8
```

**用途**
 离线应用，如文本编辑器。

#### 桥接容器

除了可以访问私有回环网络，还提供以太网接口和桥接网络连接。docker默认使用桥接模式。
 **创建一个桥接容器并测试网络**

```
docker run --rm alpine ping -w 2 8.8.8.8
```

**设置域名**
 指定一个容器的域名并查询该域名，返回的IP地址为`172.17.0.4`，这个IP地址是该容器的桥接地址

```
docker run --rm --hostname myhostname alpine nslookup myhostname
```

指定域名服务器

```
docker run --rm --dns 8.8.8.8 alpine nslookup docker.com
```

指定域名查找的后缀

```
docker run --rm --dns-search baidu.com busybox nslookup tieba
```

域名和IP映射

```
docker run --rm --add-host test:10.10.10.255 alpine nslookup test
```

**入站网络**
 桥接容器不允许容器外部网络访问容器内部网络。
 使用docker的-p命令可以实现这需求

```
# 容器动态分配端口，主机3333端口能够访问
docker run -p 3333
# 容器指定3333端口，主机3333端口能够访问
docker run -p 3333:3333
# 容器动态分配端口，特定主机3333端口能够访问
docker run -p 192.168.79.130:3333
# 容器分配3333端口，特定主机3333端口能够访问
docker run -p 192.168.79.130
```

使用-P命令暴露容器所有端口, --expose命名表示暴露指定端口

```
docker run -d --name expose-test --expose 8000 -P dockerinaction/ch5_expose
docker port expose-test
```

禁止和其他容器通信

```
docker -d --icc=false ...
```

指定连接的网桥

```
docker -d --bip "192.168.0.128"
# 或 CIDR表示无类域间路由
docker -d --fixed-cidr "192.168.0.192/26"
# 设置mtu
docker -d mtu 1200
# 创建一个网桥，这个需要对linux内核有深入的理解，知道有这么个玩意就ok了
docker -d -b mybridge
```

#### 连接容器

连接容器允许容器之间互连
 **连接容器**

```
docker run -d --name join-test --net none alpine nc -l 127.0.0.1:3333
docker run -it --net container:join-test alpine netstat -al
```

#### 开放容器

危险的容器，畅通无阻访问网络

```
docker run --rm --net host alpine ip addr
```

#### 容器内部的依赖

**使用–link命令**

## 第六章 隔离减少风险

#### 资源限制

限制内存、CPU和设备.
 **内存**

```
docker run -d --name memory-test --memory 256m --cpu-shares 1024 --user nobody --cap-drop all alpine
```

**CPU**
 使用上面提到的`--cpu-shares`分配CPU资源，资源是这么分配的。假设容器A分配512，容器B分配512，容器C分配1024。那么容器C就获取CPU一般的资源，A和B各四分之一。
 使用`--cpuset-cpus`指定CPU的核心

```
docker run -d --cpuset-cpus 0 --name cpu-core-test dockerinaction/ch6_stresser
docker run -it --rm dockerinaction/ch6_htop
docker rm -vf cpu-core-test
```

**设备**

```
docker run -it --rm --device /dev/video0:/dev/video0 ubuntu ls -al /dev
```

#### 共享内存

IPC：进程间通信
 案例：生产者消费者通信

```
# 开启生产者和消费者
docker run -d -u nobody --name ipc-producter dockerinaction/ch6_ipc -producer
docker run -d -u nobody --name ipc-consumer dockerinaction/ch6_ipc -consumer
# 打印日志，消费者拿不到生产者的消息，原因是消费者没有和生产者共享内存
docker logs ipc-producter
docker logs ipc-consumer
# 删除消费者，重新创建一个消费者，启动IPC命名空间容器
docker rm -v ipc-consumer
docker  run -d --name ipc-consumer --ipc container:ipc-producter dockerinaction/ch6_ipc -consumer
# 查看日志，这一次消费者可满意了吧
docker logs ipc-consumer
# 清理门户
docker rm -vf ipc-consumer ipc-producer
```

进程开放内存：风险大大的有，一般不推荐，–ipc host

```
docker -d --name ipc-consumer --ipc host dockerinaction/ch6_ipc -consumer
1
```

#### 理解用户

**查看容器的拥有者，如果输出空，那么说明容器的拥有者是root**

```
docker create --name bob busybox ping localhost
docker inspect bob --format "{{.Config.User}}"
```

**查看容器的拥有着的另一种方式**

```
docker run --rm --entrypoint "" busybox whoami
docker run --rm --entrypoint "" busybox id
```

**查看镜像可被哪些用户运行**

```
docker run --rm busybox awk -F: '$0=$1' /etc/passwd
```

**指定用户运行**

```
docker run --rm --user nobody busybox id
```

**指定用户和组运行：失败…**

```
# 创建一个组
groupadd -g 888 testGroup
# 用户:组
docker run --rm -u nobody:testGroup busybox id
# 用户ID:组ID
docker run --rm -u 8888:888 busybox id
```

**测试linux的权限**

```
# 创建garbage文件
echo "e=m2" > garbage
# 授权自读，并且所有者是root
chmod 600 garbage
sudo chown root:root garbage
# 使用nobody用户读取文件，访问拒绝
docker run --rm -v "$(pwd)"/garbage:/test/garbage -u nobody ubuntu cat /test/garbage
# 使用root用户读取文件，访问成功
docker run --rm -v "$(pwd)"/garbage:/test/garbage -u root ubuntu cat /test/garbage
# 垃圾回收
rm -f garbage
```

**volume有权限的文件**

```
mkdir logsFile
chown 2000:2000 logsFile
docker run --rm  -v "$(pwd)"/logsFile:/logsFile -u 2000:2000 ubuntu /bin/bash -c "echo I can do it > /logsFile/important.log"
docker run --rm -v "$(pwd)"/logsFile:/logsFile -u 2000:2000 ubuntu /bin/bash -c "echo I can't do it > /logsFile/important.log"
```

#### 能力越大，责任越大

容器具有一些能力
 **删除能力**

```
docker run --rm -u nobody ubuntu /bin/bash -c "capsh --print | grep net_raw"
docker run --rm -u nobody --cap-drop net_raw ubuntu /bin/bash -c "capsh --print | grep net_raw"
```

**增加能力**

```
docker run --rm -u nobody ubuntu /bin/bash -c "capsh --print | grep sys_admin"
docker run --rm -u nobody --cap-add sys_admin ubuntu /bin/bash -c "capsh --print | grep sys_admin"
```

#### 最高权力

**授予容器所有权限**

```
docker run --rm --privileged ubuntu id
docker run --rm --privileged ubuntu capsh -print
# 神奇吧，访问主机的资源
docker run --rm --privileged ubuntu ls /dev
docker run --rm --privileged ubuntu ifconfig
```

#### 强化容器

LXC（Linux Container

#### 测试用例

# Part 2

## 打包软件

#### 构建docker镜像

**修改容器并创建镜像**

```
docker run --name hw_container2 ubuntu touch /Helloworld
docker commit hw_container2 hw_image
docker run --rm hw_image ls -l /Helloworld

```

**在容器中安装git**

```
docker run -it --name my-image ubuntu /bin/bash
# 如果提示cannot connecting to archive.ubuntu.com，请重启服务器试试
apt-get update
apt-get -y install git
git version
exit
docker diff my-image

```

**diff命令**

```
# add
docker run --name my-busybox busybox touch /Helloworld
docker diff my-busybox
# del
docker run --name my-busybox2 busybox rm /bin/vi
docker diff my-busybox2
# edit
docker run --name my-busybox3 busybox touch /bin/vi
docker diff my-busybox3
# 删除所有
docker rm -vf my-busybox
docker rm -vf my-busybox2
docker rm -vf my-busybox3

```

**commit命令**

```
docker commit -a "@cbl" -m "The first commit" my-image my-image-git
# 使用docker images查看最新提交的镜像
docker images
# 执行失败。据说原因是之前已经用命令行打开一个镜像，再次打开的话，会再次打开命令行，会直接退出.（这块不解作者意）
```

**如果厌倦每次都需要输出git，可以把git设置成入口

```
docker run --name cmd-git --entrypoint git my-image-git
docker rm -vf cmd-git
# 省略git
docker run --name cmd-git my-image-git version
```

**继承容器的属性**

```
docker run --name inherit-test -e ENV_TEST=Hello -e ENV_TEST2=World busybox
docker commit inherit-test inherit-test-git
docker run --rm inherit-test-git /bin/sh -c "echo \$ENV_TEST \$ENV_TEST2"
# 修改镜像为：默认输出Helloworld，并且加载命令行入口
docker run --name inherit-test2 --entrypoint "/bin/sh" inherit-test-git -c "echo \$ENV_TEST \$ENV_TEST2"
docker commit inherit-test2 inherit-test-git
docker run --rm inherit-test-git
```

#### 深入理解docker镜像和层次

#### 导出导入平面文件系统

**导出Docker镜像**

```
docker run --name export-test dockerinaction/ch7_packed ./echo For Export
docker export --output contents.tar export-test
docker rm export-test
tar -tf contents.tar
```

**export命令如果不使用–output选项，则将镜像转换为tarball流，可以被其他系统import**

```
# 创建一个golang版Helloworld文件

touch HelloWorld.go
```

```
package main
import "fmt"
func main() {
 fmt.Println("hello, world!")
}
```



```
# 编译
docker run --rm -v "$(pwd)":/usr/src/hello -w /usr/src/hello golang:1.3 go build -v
# 打包
tar -cf static_hello.tar hello
# 导入
docker import -c "ENTRYPOINT [\"/hello\"]" - dockerinaction/ch7_static < static_hello.tar
# 运行
docker run dockerinaction/ch7_static
# 镜像历史记录
docker history dockerinaction/ch7_static
```

#### 版本控制最佳实践

## 自动化构建和高级镜像

#### 使用Dockerfile打包Git（如果以前执行过这些步骤，直接使用缓存，如果需要完全构建，使用–no-cache）

```
# 创建一个Dockerfile文件
mkdir dockerfiletest
cd dockerfiletest/
vim DockerFile
```

```
FROM ubuntu
MAINTAINER "dockerinaction@allingeek.com"
RUN apt-get update
RUN apt-get install -y git
ENTRYPOINT ["git"]
```



```

# 构建
docker build --tag ubuntu-git:auto .
docker images
docker run --rm ubuntu-git:auto --help
```

#### Dockerfile初步认识

**邮件程序**

```
# 创建一个ignore文件
mkdir mail
cd mail
touch .dockerignore
1234
mailer-base.df
mailer-logging.df
mailer-live.df
```

```

# 创建一个构建文件
touch mailer-base.df
vim mailer-base.df
```

```
FROM debian:wheezy
MAINTAINER Cbl "cbl@gmail.com"
RUN groupadd -r -g 2200 example && useradd -rM -g example -u 2200 example
ENV APPROOT="/app" APP="mailer.sh" VERSION="0.6"
LABEL base.name="Mailer Archetype" base.version="${VERSION}"
WORKDIR $APPROOT
ADD . $APPROOT
ENTRYPOINT ["/app/mailer.sh"]
EXPOSE 33333
```



```
# 构建
docker build -t dockerinaction/mailer-base:0.6 -f mailer-base.df .
docker inspect dockerinaction/mailer

```

**创建一个日志构建文件**

```
# 创建一个日志构建文件
touch mailer-logging.df
vim mailer-logging.df
```

```
FROM dockerinaction/mailer-base:0.6
COPY ["./log-impl", "${APPROOT}"]
RUN chmod a+x ${APPROOT}/${APP} && chown example:example /var/log
USER example:example
VOLUME ["/var/log"]
CMD ["/var/log/mailer.log"]
```

```
# 创建log-impl/mailer.sh
mkdir log-impl
cd log-impl/
touch mailer.sh
vim mailer.sh
```

```
#!/bin/sh
printf "Logging Mailer has started.\n"
while true
do
 MESSAGE=$(nc -l -p 33333)
 printf "[Message]: %s\n" "$MESSAGE" > $1
 sleep 1
done
```



```
# 构建
docker build -t dockerinaction/mailer-logging -f mailer-logging.df .
docker run -d --name logging-mailer dockerinaction/mailer-logging

```

**日志程序的另一种实现**

```
# 创建dockerfile文件
touch mailer-live.df
vim mailer-live.df
```

```
FROM dockerinaction/mailer-base:0.6
ADD ["./live-impl", "${APPROOT}"]
RUN apt-get update && apt-get install -y curl python && curl "https://b
ootstrap.pypa.io/get-pip.py" -o "get-pip.py" && python get-pip.py && pi
p install awscli && rm get-pip.py && chmod a+x "${APPROOT}/${APP}"
RUN apt-get install -y netcat
USER example:example
CMD ["mailer@dockerinaction.com", "pager@dockerinaction.com"]
```

```

mkdir live-impl
vim mailer.sh
```

```
#!/bin/sh
printf "Live Mailer has started.\n"
while true
do
 MESSAGE=$(nc -l -p 33333)
 aws ses send-email --from $1 \
 --destination {\"ToAddresses\":[\"$2\"]} \
 --message "{\"Subject\":{\"Data\":\"Mailer Alert\"},\
 \"Body\":{\"Text\":{\"Data\":\"$MESSAGE}\"}}}"
 sleep 1
done
```



```
docker build -t dockerinaction/mailer-live -f mailer-live.df .
docker run -d --name live-mailer dockerinaction/mailer-live
```

#### onbuild：下游构建时触发

**onbuild案例**

```
# 创建上游dockerfile
mkdir onbuild
cd onbuild
vim base.df
```

```
FROM busybox
WORKDIR /app
RUN touch /app/base-evidence
ONBUILD RUN ls -al /app
```

```
# 创建下游dockerfile
vim downstream.df
```

```
FROM dockerinaction/ch8_onbuild
RUN touch downstream-evidence
RUN ls -al .
```



```
# 构建上游dockerfile
docker build -t dockerinaction/ch8_onbuild -f base.df .
# 构建下游dockerfile
docker build -t dockerinaction/ch8_onbuild_down -f downstream.df .
```

#### 开始脚本和多进程容器

#### 构建哈登的镜像

**内容可寻址镜像标识符**

```
FROM debian@sha256:d5e87cfcb730
FROM debian:jessie
```

**创建用户/组并授权而不是直接使用主机的用户/组，因为作为一个容器创建者，他不可能知道用户创建了哪些用户/组**

```
RUN groupadd -r postgres && useradd -r -g postgres postgres
```

## 公私软件分发

#### 选择一个分发方法