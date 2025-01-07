### Docker常用命令

![](imgs/8ac495ea481d404a0262bf836466a604.png)

| 命令      |                             作用                             |
| --------- | :----------------------------------------------------------: |
| attach    | 绑定到运行中容器的标准输入、输出，以及错误流（这样似乎也能进入容器内容，但是一定小心，它们操作的就是控制台，控制台的退出命令会生效，比如redis、nginx） |
| build     |                从一个 Dockerfile 文件构建镜像                |
| commit    |              把容器的改变 提交创建一个新的镜像               |
| cp        |             容器和本地文件系统间复制文件/文件夹              |
| create    | 创建新容器，但并不启动(注意与docker run 的区分)需要手动启动。start\stop |
| diff      | 检查容器里文件系统结构的更改【A:添加文件或目录 D:文件或者目录删除 C:文 件或者目录更改】 |
| events    |                     获取服务器的实时事件                     |
| exec      |                   在运行时的容器内运行命令                   |
| export    | 导出 **容器** 的文件系统为一个tar文件。commit是直接提交成镜像，export是导出成文 件方便传输 |
| history   |                        显示镜像的历史                        |
| images    |                         列出所有镜像                         |
| import    | 导入tar的内容创建一个镜像，再导入进来的镜像直接启动不了容器。 /docker-entrypoint.sh nginx -g 'daemon o;' docker ps --no-trunc 看下之前的完整启动命令再用他 |
| info      |                         显示系统信息                         |
| inspect   |                   获取docker对象的底层信息                   |
| kill      |                     杀死一个或者多个容器                     |
| load      |                     从 tar 文件加载镜像                      |
| login     |                     登录Docker registry                      |
| logout    |                     退出Docker registry                      |
| logs      | 获取容器日志;容器以前在前台控制台能输出的所有内容，都可以看到 |
| pause     |                     暂停一个或者多个容器                     |
| port      |                      列出容器的端口映射                      |
| ps        |                         列出所有容器                         |
| pull      |            从registry下载一个image 或者repository            |
| push      |            给registry推送一个image或者repository             |
| rename    |                        重命名一个容器                        |
| restart   |                     重启一个或者多个容器                     |
| rm        |                     移除一个或者多个容器                     |
| rmi       |                     移除一个或者多个镜像                     |
| run       |                        创建并启动容器                        |
| save      |            把一个或者多个 **镜像** 保存为tar文件             |
| search    |                     去docker hub寻找镜像                     |
| start     |                     启动一个或者多个容器                     |
| stats     |                  显示容器资源的实时使用状态                  |
| stop      |                     停止一个或者多个容器                     |
| tag       |            给源镜像创建一个新的标签，变成新的镜像            |
| top       |                    显示正在运行容器的进程                    |
| unpause   |                        pause的反操作                         |
| update    |                更新一个或者多个docker容器配置                |
| container |                           管理容器                           |
| network   |                           管理网络                           |
| volume    |                            管理卷                            |

### Docker镜像

#### 查找镜像

- docker search [OPTIONS] TERM

```shell
#查看docker search 帮助文档
[root@jackycheung ~]# docker search --help

Usage:  docker search [OPTIONS] TERM

Search Docker Hub for images

Options:
	#根据提供条件过滤输出
  -f, --filter filter   Filter output based on conditions provided
  		#用GO模板打印指定搜索结果
      --format string   Pretty-print search using a Go template
      #搜索结果的最大数量（默认值为25）
      --limit int       Max number of search results
      #不要截断输出
      --no-trunc        Don't truncate output
```

```shell
[root@jackycheung ~]# docker search oracle
NAME                          DESCRIPTION                                     STARS     OFFICIAL
paketobuildpacks/oracle                                                       1         
vulhub/oracle                                                                 0         
softwareplant/oracle          oracle db                                       3         
datagrip/oracle               Oracle 11.2 & 12.1.0.2-se2 & 11.2.0.2-xe        24        
lidofinance/oracle                                                            0         
bostongov/oracle                                                              0         
loblaw/oracle                                                                 2         
apecloud/oracle                                                               0         
openlegacy/oracle                                                             0         
cmssw/oracle                                                                  0         
isimplelab/oracle             СУБД Oracle XE 11g для разработки и тестиров…   0         
daggerok/oracle               Oracle Database 11g Express Edition Release …   2         
switchboardlabs/oracle        Switchboard On Demand Oracle - Oracle Docker…   0         
xebialabsunsupported/oracle   Oralce DBs required for XLR and XLD             1         
hussamnoah/oracle                                                             0         
tantalusadmin/oracle                                                          0         
izone/oracle                  Oracle 11g with Debian Wheezy - 692.6 MB        1         
dbsci/oracle                  Oracle service.                                 0         
exoplatform/oracle            An Oracle database image for testing purpose.   1         
hernancasla/oracle                                                            0         
conductorboot/oracle          Oracle Database Image - 12C : Enterprise Edi…   2         
stakewiselabs/oracle                                                          0         
tqldtqmapirails/oracle                                                        0         
garland/oracle                                                                0         
dockermph/oracle                                                              0         
[root@jackycheung ~]# 
```

#### 获取镜像

- docker pull [OPTIONS] NAME[:TAG|@DIGEST]

```shell
[root@jackycheung ~]# docker pull -h   
Flag shorthand -h has been deprecated, please use --help

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Download an image from a registry

Aliases:
  docker image pull, docker pull

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output
#不指定版本号，代表laster版本
[root@jackycheung ~]# docker pull maven
Using default tag: latest
latest: Pulling from library/maven
de44b265507a: Already exists 
4c2afd91a87d: Already exists 
89e9bbcfa697: Already exists 
11be3e613582: Already exists 
1b9d1e181a2a: Already exists 
b8ebb86a487b: Pull complete 
9b375cb6232e: Pull complete 
866bd5f3675e: Pull complete 
4f4fb700ef54: Pull complete 
a2c3c2efdc18: Pull complete 
Digest: sha256:b89ede2635fb8ebd9ba7a3f7d56140f2bd31337b8b0e9fa586b657ee003307a7
Status: Downloaded newer image for maven:latest
docker.io/library/maven:latest
```

#### 列出镜像

- docker images
- docker image ls 

```shell
[root@jackycheung ~]# docker images 
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
redis         latest    691a00f92e2c   11 hours ago    117MB
tomcat        latest    f62f518e5c5c   4 weeks ago     467MB
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
mysql         latest    56a8c14e1404   2 months ago    603MB
maven         latest    08eeb946c6cf   4 months ago    530MB
httpd         latest    4ce47c750a58   5 months ago    147MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
[root@jackycheung ~]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
redis         latest    691a00f92e2c   11 hours ago    117MB
tomcat        latest    f62f518e5c5c   4 weeks ago     467MB
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
mysql         latest    56a8c14e1404   2 months ago    603MB
maven         latest    08eeb946c6cf   4 months ago    530MB
httpd         latest    4ce47c750a58   5 months ago    147MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
```

#### 删除本地镜像

- docker rmi [OPTIONS] IMAGE [IMAGE...]

```shell
#docker image rmi:指定版本号
[root@jackycheung ~]# docker image rmi maven
Untagged: maven:latest
Untagged: maven@sha256:b89ede2635fb8ebd9ba7a3f7d56140f2bd31337b8b0e9fa586b657ee003307a7
Deleted: sha256:08eeb946c6cfd01149e6111337f8e0cec734d3ab7723188d8f69b5d6b8de9887
Deleted: sha256:73249cda3f92c07a62cf91264ec7f56e886a45ccf7d723bc8f05cb9d413c4749
Deleted: sha256:8d063f7f456059a028ae9d906ba2cc06248e0aa7fe45fdb0a16372b8860bbb96
Deleted: sha256:2e97cef601719b4c783ddb4275c48191eeb9350ce4db5ebb5ef5addeb9610125
Deleted: sha256:86d5fca4348dc73cce790906700c0afdc34c2f4cc69bf3a3eea933747eb0afc8
Deleted: sha256:971c5e7f186b411223bffeabd5921effc0498c16994e22c58b0a856d2b0a7d13
[root@jackycheung ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
redis         latest    691a00f92e2c   11 hours ago    117MB
tomcat        latest    f62f518e5c5c   4 weeks ago     467MB
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
mysql         latest    56a8c14e1404   2 months ago    603MB
httpd         latest    4ce47c750a58   5 months ago    147MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
```

#### 查看本地镜像的IMAGE ID

```shell
[root@jackycheung ~]# docker images -q
691a00f92e2c
f62f518e5c5c
f876bfc1cc63
56a8c14e1404
4ce47c750a58
d2c94e258dcb
```

#### 查看镜像制作历史

- docker history 镜像名

```shell
[root@jackycheung ~]# docker history redis
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
691a00f92e2c   11 hours ago   CMD ["redis-server"]                            0B        buildkit.dockerfile.v0
<missing>      11 hours ago   EXPOSE map[6379/tcp:{}]                         0B        buildkit.dockerfile.v0
<missing>      11 hours ago   ENTRYPOINT ["docker-entrypoint.sh"]             0B        buildkit.dockerfile.v0
<missing>      11 hours ago   COPY docker-entrypoint.sh /usr/local/bin/ # …   661B      buildkit.dockerfile.v0
<missing>      11 hours ago   WORKDIR /data                                   0B        buildkit.dockerfile.v0
<missing>      11 hours ago   VOLUME [/data]                                  0B        buildkit.dockerfile.v0
<missing>      11 hours ago   RUN /bin/sh -c mkdir /data && chown redis:re…   0B        buildkit.dockerfile.v0
<missing>      11 hours ago   RUN /bin/sh -c set -eux;   savedAptMark="$(a…   38.1MB    buildkit.dockerfile.v0
<missing>      11 hours ago   ENV REDIS_DOWNLOAD_SHA=4ddebbf09061cbb589011…   0B        buildkit.dockerfile.v0
<missing>      11 hours ago   ENV REDIS_DOWNLOAD_URL=http://download.redis…   0B        buildkit.dockerfile.v0
<missing>      11 hours ago   ENV REDIS_VERSION=7.4.2                         0B        buildkit.dockerfile.v0
<missing>      11 hours ago   RUN /bin/sh -c set -eux;  savedAptMark="$(ap…   4.12MB    buildkit.dockerfile.v0
<missing>      11 hours ago   ENV GOSU_VERSION=1.17                           0B        buildkit.dockerfile.v0
<missing>      11 hours ago   RUN /bin/sh -c set -eux;  apt-get update;  a…   5.08kB    buildkit.dockerfile.v0
<missing>      11 hours ago   RUN /bin/sh -c set -eux;  groupadd -r -g 999…   4.3kB     buildkit.dockerfile.v0
<missing>      2 weeks ago    # debian.sh --arch 'amd64' out/ 'bookworm' '…   74.8MB    debuerreotype 0.15
[root@jackycheung ~]# 
```

#### 保存镜像

- docker save [OPTIONS] IMAGE [IMAGE...]：save选项将本地仓库的镜像保存当前目录下

- docker load [OPTIONS]：load -i选项将打包的镜像文件导入到本地Docker仓库

```shell
[root@jackycheung ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
redis         latest    691a00f92e2c   11 hours ago    117MB
tomcat        latest    f62f518e5c5c   4 weeks ago     467MB
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
mysql         latest    56a8c14e1404   2 months ago    603MB
httpd         latest    4ce47c750a58   5 months ago    147MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
# 保存到当前目录下
[root@jackycheung ~]# docker save -o hello-world.tar hello-world
[root@jackycheung ~]# ll hello-world.tar 
-rw------- 1 root root 25600 Jan  7 11:08 hello-world.tar
[root@jackycheung ~]# docker image rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:5b3cc85e16e3058003c13b7821318369dad01dac3dbb877aac3c28182255c724
Deleted: sha256:d2c94e258dcb3c5ac2798d32e1249e42ef01cba4841c2234249495f87264ac5a
Deleted: sha256:ac28800ec8bb38d5c35b49d45a6ac4777544941199075dff8c4eb63e093aa81e
[root@jackycheung ~]# docker images |grep hello-world
[root@jackycheung ~]# docker load -i hello-world.tar 
ac28800ec8bb: Loading layer  14.85kB/14.85kB
Loaded image: hello-world:latest
[root@jackycheung ~]# docker images |grep hello-world
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
```

#### 虚悬镜像（Dangling Image)

镜像列表中，有一种特殊的镜像，这个镜像既没有仓库名，也没有标签，称为虚悬镜像。

- docker image prune：删除虚悬镜像

> 这个镜像原本是有镜像名和标签的，原来为 tomcat:8.0，随着官方镜像维护，发布了新版本后，重新 docker pull tomcat:8.0 时，tomcat:8.0 这个镜像名被转移到了新下载的镜像身上，而旧的镜像上的这个名称则被取消， 从而成为了 。除了 docker pull 可能导致这种情况，docker build 也同样可以导致这种现象。由于新旧镜像同名，旧镜像名称被取消，从而出现仓库名、标签均为 的镜像。这类无标签镜像也被称为虚悬镜像(dangling image) 。 一般来说，虚悬镜像已经失去了存在的价值，是可以随意删除的.

### Docker容器

#### 启动容器

- docker run 参数 镜像名称:tag 执行命令

- docker create [OPTIONS] IMAGE [COMMAND] [ARG...]   #create创建好容器不会启动，需要手动启动

**常用参数：**

- -i  #保持和docker容器内的交互，启动容器时，运行的命令结束后，容器依然存活，没有退出（默认是会退出，即停止）
- -t  #为容器提供标准输入虚拟的一个tty
- -d  #后台运行容器
- --rm  #容器在启动后，执行完成命令或程序后就销毁
- --name  # 给容器起一个名称
- -p 宿主机端口:内部端口  #宿主机和容器内部端口映射

```shell
[root@jackycheung ~]# docker create redis
f0475670562884e4f78d395acea43d55f3ce293fe83c7f7c8cac7999cd22c33d
[root@jackycheung ~]# docker ps -a |grep redis
f04756705628   redis     "docker-entrypoint.s…"   9 seconds ago   Created             blissful_carver
[root@jackycheung ~]# docker run -d -p 6379:6379 --name redis2 redis
39bba36740fc468526f196dd5fa9af3046e6eb1cd0dd16ba0444f12842644e47
[root@jackycheung ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
39bba36740fc   redis     "docker-entrypoint.s…"   12 seconds ago   Up 11 seconds   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
```

#### 查看容器状态

- docker ps  #查看正在运行的容器
- docker ps -a   #查看所有容器（包含运行和停止）
- docker container ls
- docker container ls -a

```shell
[root@jackycheung ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
39bba36740fc   redis     "docker-entrypoint.s…"   54 minutes ago   Up 54 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
[root@jackycheung ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
39bba36740fc   redis     "docker-entrypoint.s…"   54 minutes ago   Up 54 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
f04756705628   redis     "docker-entrypoint.s…"   56 minutes ago   Created                                                     blissful_carver
[root@jackycheung ~]# docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
39bba36740fc   redis     "docker-entrypoint.s…"   54 minutes ago   Up 54 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
[root@jackycheung ~]# docker container ls -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
39bba36740fc   redis     "docker-entrypoint.s…"   54 minutes ago   Up 54 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
f04756705628   redis     "docker-entrypoint.s…"   56 minutes ago   Created                                                     blissful_carver
```

#### 停止容器

- docker stop 容器名
- docker container stop 容器名
- docker stop $(docker ps  -q)

```shell
[root@jackycheung ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
3ac3ec680cf9   redis     "docker-entrypoint.s…"   18 seconds ago   Up 17 seconds   0.0.0.0:6380->6379/tcp, :::6380->6379/tcp   redis3
39bba36740fc   redis     "docker-entrypoint.s…"   57 minutes ago   Up 6 seconds    0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
[root@jackycheung ~]# docker stop redis2
redis2
[root@jackycheung ~]# docker container stop redis2
redis2
[root@jackycheung ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS                                       NAMES
3ac3ec680cf9   redis     "docker-entrypoint.s…"   42 seconds ago   Up 41 seconds               0.0.0.0:6380->6379/tcp, :::6380->6379/tcp   redis3
39bba36740fc   redis     "docker-entrypoint.s…"   58 minutes ago   Exited (0) 15 seconds ago                                               redis2
f04756705628   redis     "docker-entrypoint.s…"   59 minutes ago   Created                                                                 blissful_carver

```

#### 启动已停止容器

- docker start 容器id

```shell
[root@jackycheung ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS                          PORTS     NAMES
3ac3ec680cf9   redis     "docker-entrypoint.s…"   3 minutes ago       Exited (0) About a minute ago             redis3
39bba36740fc   redis     "docker-entrypoint.s…"   About an hour ago   Exited (0) 2 minutes ago                  redis2
f04756705628   redis     "docker-entrypoint.s…"   About an hour ago   Created                                   blissful_carver
[root@jackycheung ~]# docker start 3ac3e 39bba
3ac3e
39bba
[root@jackycheung ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS         PORTS                                       NAMES
3ac3ec680cf9   redis     "docker-entrypoint.s…"   3 minutes ago       Up 4 seconds   0.0.0.0:6380->6379/tcp, :::6380->6379/tcp   redis3
39bba36740fc   redis     "docker-entrypoint.s…"   About an hour ago   Up 3 seconds   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   redis2
```

#### 重启容器

- docker restart 容器id

```shell
[root@jackycheung ~]# docker restart 3ac3e
3ac3e
```

#### 删除容器

- docker rm 容器id	#删除容器需要停止容器

```shell
[root@jackycheung ~]# docker stop redis3
redis3
[root@jackycheung ~]# docker rm redis3
redis3
[root@jackycheung ~]# docker ps -a |grep redis3
[root@jackycheung ~]# 
```

#### 查看后台运行日志

- docker logs 容器id/容器名

```shell
[root@jackycheung ~]# docker logs redis2
```

#### 进入容器

- docker exec -it 容器id/容器名 bash

```shell
[root@jackycheung ~]# docker exec -it redis2 bash
root@39bba36740fc:/data# exit
exit
```

> 默认容器内linux包是最小安装。只提供最基本的命令；exit不会导致容器的停止。

#### 宿主机和容器之间交换文件

- docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH	#容器复制到宿主机
- docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH    #宿主机复制到容器

```shell

```



















```shell
#查看docker镜像
[root@jackycheung /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
[root@jackycheung /]# docker run -d nginx
8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c
#查看所有（运行和停止）的容器
[root@jackycheung /]# docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS                   PORTS     NAMES
8759cfb78183   nginx         "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds             80/tcp    unruffled_volhard
f67bd7cdb3e7   hello-world   "/hello"                 4 hours ago     Exited (0) 4 hours ago             youthful_clarke
#查看正在运行的容器
[root@jackycheung /]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
8759cfb78183   nginx     "/docker-entrypoint.…"   16 seconds ago   Up 15 seconds   80/tcp    unruffled_volhard
#查看容器结构信息
[root@jackycheung /]# docker inspect 8759
[
    {
        "Id": "8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c",
        "Created": "2025-01-06T08:29:19.610806941Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3098,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2025-01-06T08:29:19.935637493Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:f876bfc1cc63d905bb9c8ebc5adc98375bb8e22920959719d1a96e8f594868fa",
        "ResolvConfPath": "/var/lib/docker/containers/8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c/hostname",
        "HostsPath": "/var/lib/docker/containers/8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c/hosts",
        "LogPath": "/var/lib/docker/containers/8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c/8759cfb7818322c0b7e1171bcb2433d4cf96e0dbd90298f236317b8e903f2c3c-json.log",
        "Name": "/unruffled_volhard",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                23,
                75
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
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
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": [],
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
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
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
                "LowerDir": "/var/lib/docker/overlay2/f2e4b6123da19e09d9a3a942054f4096151aaf2d3cca8ac56892aaa23a1b0456-init/diff:/var/lib/docker/overlay2/9bb05466a0f36efc75bff7af553988cbb50ffacdaa583fea1eb71e74be0f4114/diff:/var/lib/docker/overlay2/a61d59a0c532ffd9a378e8691729d051aaa246a5991d94932fabeb6085e62c40/diff:/var/lib/docker/overlay2/a24178cbc1b84cfa37f190896f473ea877546bdf7ed0e7d63857f255ae73e4f7/diff:/var/lib/docker/overlay2/545e6297b9baec23854ac5285b9296d6569e374bbc0041572e4a05abefc4ce29/diff:/var/lib/docker/overlay2/80aaa894866b580b9e7435c5ea341cee65320a9bee492cddb04edc6f9a0863ff/diff:/var/lib/docker/overlay2/ca0ac03ef4f6a0f8f579e3753e50a4628b606377fd7d13441ab11f14d8b3d4a8/diff:/var/lib/docker/overlay2/6c90847c8ca2808c2bcdebf6bf6ebfe23cf08e7460d63ca558c46645f1383472/diff",
                "MergedDir": "/var/lib/docker/overlay2/f2e4b6123da19e09d9a3a942054f4096151aaf2d3cca8ac56892aaa23a1b0456/merged",
                "UpperDir": "/var/lib/docker/overlay2/f2e4b6123da19e09d9a3a942054f4096151aaf2d3cca8ac56892aaa23a1b0456/diff",
                "WorkDir": "/var/lib/docker/overlay2/f2e4b6123da19e09d9a3a942054f4096151aaf2d3cca8ac56892aaa23a1b0456/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "8759cfb78183",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "26ec15030238c9ffa57fc5786ed32b49f2e2369c2eb332849d241a9f704797aa",
            "SandboxKey": "/var/run/docker/netns/26ec15030238",
            "Ports": {
                "80/tcp": null
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "7c5184eebdae5720cfd148f2bc26f2d36a260bf72ee0161b10850b51d059ddc9",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "NetworkID": "5998d75979273770a0169835e01575f59800cc7d561d67cce07b78fd7be229cd",
                    "EndpointID": "7c5184eebdae5720cfd148f2bc26f2d36a260bf72ee0161b10850b51d059ddc9",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DriverOpts": null,
                    "DNSNames": null
                }
            }
        }
    }
]
#docker0 网桥，用于容器提供桥接，转发到主机之外的网络
[root@jackycheung /]# ip a s
........
5: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:f3:26:2e:8a brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:f3ff:fe26:2e8a/64 scope link 
       valid_lft forever preferred_lft forever
#与容器中的虚拟网络设备在同一个命名空间中，用于把容器的网络连接到主机   
7: veth4150726@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether a6:61:6f:8e:c5:21 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::a461:6fff:fe8e:c521/64 scope link 
       valid_lft forever preferred_lft forever
#访问nginx服务
[root@jackycheung /]# curl 172.17.0.2
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

```shell
[root@jackycheung ~]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
fd674058ff8f: Already exists 
e1fbcf8737cd: Pull complete 
11099e00141f: Pull complete 
847699972819: Pull complete 
66d3cac5d3d8: Pull complete 
45cb8d36c900: Pull complete 
4f4fb700ef54: Pull complete 
42bec219b12d: Pull complete 
Digest: sha256:cd13b924409d740ea8abe6677a7d1accf696898408d330a3d7c8234fa7545775
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
[root@jackycheung ~]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
redis         latest    691a00f92e2c   10 hours ago    117MB
tomcat        latest    f62f518e5c5c   4 weeks ago     467MB
nginx         latest    f876bfc1cc63   5 weeks ago     192MB
mysql         latest    56a8c14e1404   2 months ago    603MB
httpd         latest    4ce47c750a58   5 months ago    147MB
hello-world   latest    d2c94e258dcb   20 months ago   13.3kB
```

