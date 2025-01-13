### 容器镜像的分类

- 操作系统类
  - Centos
  - Ubuntu
- 应用类
  - tomcat
  - nginx
  - mysql
  - redis

### 容器镜像获取的方法

- DockerHub
- 把操作系统打包为容器镜像
- 把正在运行的容器打包为镜像，即commit
- 通过Dockerfile实现容器镜像的自定义及生成

### 把操作系统中文件系统打包为容器镜像

```shell
#把操作系统中文件系统进行打包
tar --numeric-owner --exclude=/proc --exclude=/sys -cvf centos7u6.tar /
scp -P 10022 centos7-1810.tar root@192.168.194.10:/root/

#把打包的centos文件系统生成本地容器镜像 
[root@docker ~]# docker import centos7-1810.tar centos7-1810:v1
sha256:f5f6bc8e192d1085cda0aabc1aac78a7eec41098ad55e27f6bdc38f105e5c540
[root@docker ~]# docker images
REPOSITORY                     TAG          IMAGE ID       CREATED              SIZE
centos7-1810                   v1           f5f6bc8e192d   About a minute ago   1.09GB
#运行镜像
[root@docker ~]# docker run -it centos7-1810:v1 bash
[root@562bf0a52b79 /]# ip a s
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
10: eth0@if11: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

### Dockerfile

Dockerfile是一种能够被Docker程序解释的剧本。Dockerfile由一条一条的指令组成，并且有自己的书写格式和支持的命令。当我们需要在容器镜像中指定自己额外的需求时，只需在Dockerfile上添加或修改指令，然后通过docker build生成我们自定义的容器镜像（image）。

![](imgs/clip_image002-1644469873598.jpg)

- Docker镜像的最基本组成部分是Dockerfile

- Dockerfile是带有指令和参数的简单文本文件。Docker可以通过读取Dockerfile中给出的指令来自动构建镜像。

- 在Dockerfile中，左侧的所有内容都是指令，右侧是这些指令的参数。注意：Dockerfile文件没有任何扩展名

![](imgs/image-4.png)

#### Dockerfile指令

- 构建类指令
  - 用于构建image
  - 指定的操作不会在运行image的容器上执行（FROM、MAINTAINER、RUN、ENV、ADD、COPY）
- 设置类指令
  - 用于设置image的属性
  - 指定的操作将在运行image的容器中执行（CMD、ENTRYPOINT、USER、EXPOSE、VOLUME、WORKDIR、ONBUILD）

| 指令        | 说明                                                     | 示例                                                     |
| ----------- | -------------------------------------------------------- | -------------------------------------------------------- |
| FROM        | 指定基础镜像，所有其他指令都将在该基础上运行。           | `FROM python:3.9-slim`                                   |
| MAINTAINER  | 指定镜像维护者的信息（已被 LABEL 取代）。                | `MAINTAINER you@example.com`                             |
| WORKDIR     | 设置工作目录，所有接下来的指令都将在此目录执行。         | `WORKDIR /app`                                           |
| RUN         | 执行命令，通常用于安装包或依赖。                         | `RUN pip install --no-cache-dir -r requirements.txt`     |
| COPY        | 将文件或目录从主机复制到镜像的文件系统中。               | `COPY . /app`                                            |
| ADD         | 类似于 COPY，但能解压 tar 文件和复制 URL。               | `ADD https://example.com/file.tar.gz /app/file.tar.gz`   |
| CMD         | 指定启动容器时的默认命令或参数。                         | `CMD ["python", "app.py"]`                               |
| ENTRYPOINT  | 配置容器启动时运行的命令，允许后续传递参数。             | `ENTRYPOINT ["python", "app.py"]`                        |
| ENV         | 设置环境变量。                                           | `ENV NAME World`                                         |
| EXPOSE      | 声明镜像内部的服务监听端口。                             | `EXPOSE 80`                                              |
| VOLUME      | 创建一个挂载点，宿主机和容器之间进行持久化数据共享。     | `VOLUME /data`                                           |
| USER        | 指定运行容器时的用户名或 UID。                           | `USER appuser`                                           |
| LABEL       | 为镜像添加元数据。                                       | `LABEL maintainer="you@example.com"`                     |
| STOPSIGNAL  | 设置容器停止时发送的系统调用信号。                       | `STOPSIGNAL SIGKILL`                                     |
| ARG         | 定义可在 build 时传递的变量。                            | `ARG build_version=1.0`                                  |
| ONBUILD     | 定义触发镜像构建时执行的命令，当继承 Dockerfile 时使用。 | `ONBUILD RUN apt-get update`                             |
| HEALTHCHECK | 指定如何测试某个容器以确保它仍然正常工作。               | `HEALTHCHECK CMD curl --fail http://localhost || exit 1` |
| SHELL       | 指定 RUN、CMD 及 ENTRYPOINT 命令的默认 shell。           | `SHELL ["powershell", "-command"]`                       |

#### Dockerfile指令解释

- man dockerfile   # 用于查看详细说明

##### FROM

- FROM指令用于指定其构建新镜像所使用的基础镜像
- FROM指令必须是在Dockerfile文件中的首条命令
- FROM指令指定的基础image可以是官方远程仓库中的，也可以位于本地仓库，优先本地仓库

```dockerfile
FROM <image>:<tag>
FROM centos:latest
```

##### RUN

RUN指令用于在构建镜像中执行命令

```dockerfile
#shell格式
RUN <命令>
RUN echo 'hello docker' > /var/www/html/index.html

#exec格式
RUN ["可执行文件", "参数1","参数2"]
RUN ["/bin/bash", "-c","echo hello > /var/www/html/index.html"]
```

> 注意：按照优化的角度来讲，当有多条要执行的命令，不要使用多条RUN，尽量使用&&符号于 \ 符号连接成一行。因为多条RUN命令会让镜像建立多层

```dockerfile
RUN yum install httpd -y
RUN echo test > /var/www/html/index.html
#改成
RUN yum install httpd -y && echo test > /var/www/html/index.html
```

##### CMD

CMD不同于RUN，CMD用于指定在容器启动时所要执行的命令，而RUN用于指定镜像构建要执行的命令。

```dockerfile
#格式有三种
CMD ["executable","param1","param2"]
CMD ["param1","param2"]
CMD command param1 param2
```

每个Dockerfile只能有一条CMD命令。如果指定了多条命令，只有最后一条被执行。如果用户启动容器时候指定了运行的命令，则会覆盖掉CMD指定的命令

```shell
docker run -d -p 80:80 镜像名 运行的命令
```

##### EXPOSE

EXPOSE指令用于指定容器在运行时监听的端口

```dockerfile
EXPOSE <port> [<port>....]
EXPOSE 80 3306 8080
```

还需要使用docker run运行容器时通过-p参数映射到宿主机的端口；-P随机映射到宿主机的端口

##### ENV

ENV指令用于指定一个环境变量

```dockerfile
#格式1
ENV <key> <value> 
#格式2
ENV <key>=<value>
ENV JAVA_HOME /usr/local/jdk1.8/
```

##### ADD

ADD指令用于把宿主机上的文件拷贝到镜像中

```dockerfile
# src可以是本地文件或压缩包文件，还可以是一个url
# desc路径的填写可以是容器的绝对路径，也可以是相对于工作目录的相对路径
ADD <src> <desc>
```

##### ENTRYPOINT

ENTRYPOINT与CMD非常类似

相同点：

- 一个Dockerfile只写一条，如果写了多条，那么只有最后一条生效
- 都是容器启动时才运行

不同点：

- 如果用户启动容器的时候指定了运行的命令，ENTRYPOINT不会被运行的命令覆盖，而CMD则会被覆盖

```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT commadn param1 param2
```

##### VOLUME

VOLUME指令用于把宿主机里的目录与容器的目录映射

只指定挂载点，docker宿主机映射的目录为自动生成的

```dockerfile
VOLUME ["<mountpoint>"]
```

##### USER

USER指令设置启动容器的用户（像hadoop需要hadoop用户操作，oracle需要oracle用户操作），可以是用户名或UID

```dockerfile
USER daemon
USER 1001
```

> 注意：如果设置了容器以daemon用户运行，那么RUN、CMD和ENTRYPOINT都会以这个用户去运行镜像构建完成后，通过docker run运行容器时，可以通过 -u 参数来覆盖所指定的用户

##### WORKDIR

WORKDIR指令设置工作目录，类似于cd命令。不建议使用 RUN cd /root，建议使用WORKDIR

```dockerfile
WORKDIR /root
```

#### Dockerfile基本构成

- 基础镜像信息
- 维护者信息
- 镜像操作指令
- 启动容器时执行指令

#### Dockerfile生成容器镜像方法

![](imgs/dockerfile001.png)

#### 使用Dockerfile构建Docker镜像

下面显示了镜像构建过程的高级工作流程。

<img src="imgs/docker-build-workflow.png" style="zoom:67%;" />

#### Dockerfile生成Nginx容器镜像

```shell
[root@docker ~]# mkdir nginx-image && cd nginx-image
[root@docker nginx-image]# touch .dockerignore
[root@docker nginx-image]# mkdir files
[root@docker nginx-image]# cd files/
[root@docker files]# vim index.html
[root@docker files]# cat index.html 
<html>
  <head>
    <title>Dockerfile</title>
  </head>
  <body>
    <div class="container">
      <h1>My App</h1>
      <h2>This is my first app</h2>
      <p>Hello everyone, This is running via Docker container</p>
    </div>
  </body>
</html>
[root@docker files]# vi default
[root@docker files]# cat default 
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /usr/share/nginx/html;
    index index.html index.htm;

    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
}
[root@docker nginx-image]# cat Dockerfile 
FROM centos:centos7
LABEL maintainer="canvs@qq.com"
RUN curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
RUN yum -y install nginx
COPY files/default /etc/nginx/sites-available/default
ADD files/index.html /usr/share/nginx/html/
EXPOSE 80
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```

