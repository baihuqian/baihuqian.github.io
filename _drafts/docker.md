---
layout: "post"
title: "Docker"
date: "2018-04-16 00:18"
---

{:toc}

# 什么是Docker
Docker是一个开源项目，诞生于2013年初，最初是dotCloud公司内部的一个业余项目，用Go语言实现。

Docker项目的目标是事先轻量级的操作系统虚拟化解决方案。Docker的基础是Linux容器等技术。容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统方式则是硬件层面的实现。

Docker的优点：
* 启动时间为秒级。
* 资源占用少，一台主机可以运行数千个Docker容器。
* Overhead低。容器除了运行其中应用外，基本不消耗额外的系统资源。

Docker的优势：
* 更快的交付和部署。一次开发可以在任何地方运行。
* 更高的运行效率。
* 更轻松的迁移和扩展。Docker容器可以在几乎所有的平台上运行。
* 更简单的管理。所有的修改都以增量的方式分发和更新。

Docker分成两部分：Docker engine（daemon）和可以从命令行访问的客户端工具。Docker engine提供了Docker Remote API(REST)，而命令行上的docker命令则是通过访问API来实现操作的。

## Docker Image
镜像是一个只读的模板。镜像可以用来创建Docker容器。镜像的ID唯一地标识了镜像。如果两个镜像Tag指向同一个ID则说明它们是一样的。Docker运行容器前需要本地存在对应的镜像。如果不存在则会从镜像仓库下载（默认Docker Hub）。

* 从仓库获取镜像：`docker pull (<registry_url>)/<repo_name>:<tag>`。如果Registry被省略，默认的Docker Registry会被使用。在获取一个镜像的同时，所有的依赖层都会被获取。
* 显示本地镜像：`docker images`。默认（未指定）的标记为latest。该命令有强大的查找功能，使用`--filter`或者`-f`。输出结果可以使用Go的模板语法，例如：`docker images --format "{{.ID}}:{{.Repository}}`。
* 在仓库中共享镜像：`docker push`.
* 显示镜像的历史纪录：`docker history <标签>`

### 创建镜像
用户可以在已有镜像基础上进行修改，也可以用本地文件系统创建一个。

#### 修改已有镜像
* 使用已有镜像启动一个容器，并记住容器的ID。
* 在容器内修改。结束后使用`exit`退出。
* 使用`docker commit <容器ID或名字> <仓库名>:<标签>`将容器的存储层保存下来成为镜像。

然而，使用`docker commit`创建的镜像会有很多不必要的文件改动产生，而且改动操作都是黑箱操作。另外，每次更改镜像都会新增一层，这使得镜像更加臃肿。简而言之，该方法不应该被用来创建镜像，而只是用来学习和紧急情况下的现场保存。

#### 利用Dockerfile创建镜像
`docker build`可以创建一个新的镜像。为此，首先需要创建一个Dockerfile，包含一些如何创建镜像的指令。Dockerfile的每一条指令都创建镜像的一层。创建过程中每一步都创建一个新的容器，在容器中执行指令并commit修改。注意一个镜像不能超过127层。

Dockerfile的基本语法：
* 使用`#`来注释。
* 最开始的`FROM`指令告诉Docker使用哪个镜像作为基础。它是Dockerfile中必备而且必须是第一条指令。如果不需要基础镜像，则使用`scratch`来代表空白镜像。
* 维护者信息：`MAINTAINER Baihu Qian <baihuqian@gmail.com>`
* `RUN`用来执行命令行程序。像shell一样可以使用`&&`来连接多个命令，用`\`来换行。
 * shell模式：`RUN <命令>`。在执行时会被包装成`sh -c`的参数形式，所以环境变量可以使用。但是要注意该命令必须是前台执行。如果该命令执行结束则容器退出。**注意命令不用双引号！**
 * exec模式：`CMD ["executable", "arg1", "arg2"]`。列表务必使用双引号，因为在解析时会被解析为JSON数组。
* `COPY`拷贝文件至镜像。源文件必须是基于上下文路径的相对路径，并可以使用符合Go规范的通配符；目标路径可以是容器的绝对路径也可以是相对于工作目录（用WORKDIR指定）的相对路径。使用`COPY`时文件的所有元数据（权限等）都会保留。
* `ADD`是更高级的`COPY`，可以进行文件下载（源文件是URL时），tar解压缩到目标路径（源文件是tar包时）。*`ADD`应当仅在解压缩时使用，其他场合使用`COPY`。*
* `EXPOSE`声明外部端口。注意这只是声明，而不是进行具体的端口映射。
* `CMD`描述容器启动时运行的程序。该命令可以在启动容器时被重新指定。和`RUN`一样，`CMD`有shell和exec两种模式。
* `ENTRYPOINT`入口点。同样有shell和exec模式。在指定ENTRYPOINT以后，CMD将变成参数传给ENTRYPOINT的指令，这样无论CMD是什么（运行时指定），必要的初始化都能完成。
* `ENV`设置环境变量。`ENV <key1> <value1>`或者`ENV <key1>=<value1> <key2>=<value2>`，用`\`换行。定义后后续的指令中就可以使用该变量。
* `ARG`设置构建环境的环境变量。这些变量在由镜像创建的容器中是不存在的。
* `VOLUME ["path1", "path2"]`定义匿名卷。这样向容器内这些目录写入数据时，数据将存储在卷中。运行时可以覆盖这些设置。
* `WORKDIR <path>`指定工作目录。指定后该层及以上层的工作目录都改为该目录。该目录需要事先存在。
* `USER <user>`指定当前用户。和`WORKDIR`一样，它对当前层和以上层都起作用，而且用户必须事先存在。
* `HEALTHCHECK [option] CMD <cmd>`指定如何进行健康检查。如果没有额外的健康检查，容器只能通过判断主进程是否退出来判断异常。`HEALTHCHECK NONE`屏蔽已有的健康检查。健康检查指令返回0为健康，1为失败。健康检查的输出可以用`docker inspect`查看。一个Dockerfile只能有一个HEALTHCHECK，若有多个则最后一个生效。选项：
 * `--interval`：默认为30秒。
 * `--timeout`：默认为30秒。
 * `--retries`：默认为3次。
* `ONBUILD`后跟其他指令：这些指令将在该镜像被作为基础镜像创建其它镜像时执行。


完成后使用`docker build -t="tag" <context_directory>`创建镜像。由于Docker的client-server架构，该路径将被打包上传至docker engine，并且可以被docker engine所访问。所以`COPY`命令的源文件相对路径是相对这个上下文路径来说的，任何在这个路径之外的文件都无法被访问。如果有些文件不想传给docker engine，可以像`.gitignore`一样写一个`.dockerignore`。

`docker build`的其它用法：
* 从git repo创建：`docker build <git-url>#:<folder>`
* 从tar包创建：`docker build <url-to-tar>`。docker会下载，解压缩，并以其作为上下文创建镜像。
* 从标准输入读取Dockerfile
* 从标准输入读取压缩文件

`docker tag <id> <new_tag>` 可以用来修改镜像的标签。

### 导入和导出镜像
`docker save -o <file_name> <tag>`将镜像保存到本地文件。

`docker load --input <filename>`将导出到文件的镜像导入到本地镜像库。

### 删除本地镜像
`docker rmi <tag>`删除存在于本地的docker镜像。注意`docker rm`是删除容器，在删除镜像前要用它删除所有依赖于该镜像的容器。

有的时候本地会有很多没有打过标签的镜像，用下列命令清除它们：
```bash
docker rmi $(docker images -q -f "dangling=true")
```
其中`-q`是quite，`-f`是filter。

### 镜像的实现原理
Docker镜像由很多层次构成，Docker使用Union FS将不同层结合到一个镜像中。

>Unionfs is a filesystem service for Linux, FreeBSD and NetBSD which implements a union mount for other file systems. It allows files and directories of separate file systems, known as branches, to be transparently overlaid, forming a single coherent file system. Contents of directories which have the same path within the merged branches will be seen together in a single merged directory, within the new, virtual filesystem.

>When mounting branches, the priority of one branch over the other is specified. So when both branches contain a file with the same name, one gets priority over the other.

>The different branches may be either read-only and read-write file systems, so that writes to the virtual, merged copy are directed to a specific real file system. This allows a file system to appear as writable, but without actually allowing writes to change the file system, also known as copy-on-write. This may be desirable when the media is physically read-only, such as in the case of Live CDs.

因此，镜像并非是像ISO一样的打包文件，而是一个虚拟的概念。其具体实现并非由一个文件组成，而是由多层文件系统联合组成。

镜像的每一层都是后一层的基础。每一层构建完毕后就不发生任何改变，后一层上的任何改变都只发生在该层。因此，删除前一层的文件时仅在当前层标记该文件已经删除，而被删除的文件将一直都跟随着这个镜像。所以每一层需要尽量仅包含该层需要的东西。

## Docker Container
容器是从镜像创建的运行实例。它可以被启动、开始、停止、删除。容器之间是相互隔离的，相当于一个简易版的Linux环境和运行在其中的应用程序。

容器的实质是进程。但与直接在宿主机上执行的进程不同，容器进程运行于属于自己的独立命名空间（namespace）：

>Namespaces are a feature of the Linux kernel that isolates and virtualizes system resources of a collection of processes. Examples of resources that can be virtualized include process IDs, hostnames, user IDs, network access, interprocess communication, and filesystems. Namespaces are a fundamental aspect of containers on Linux.

>Linux developers use the term namespace to refer to both the namespace kinds, as well as to specific instances of these kinds.

>A Linux system is initialized with a single instance of each namespace type. After initialization, additional namespaces can be created or joined.

所以容器可以拥有自己的root文件系统、网络配置、进程空间、用户ID空间。所以容器内的进程是运行在一个隔离的环境里，使得容器封装的应用比直接在宿主上运行更加安全。

和镜像一样，容器也是分层存储的。容器在启动时会创建一层可写层作为最上层，称为**容器存储层**。该层的生命周期同容器一样，随着容器的删除而丢失。按照Docker最佳实践的要求，*容器不应该向其存储层内写入任何数据，而应保持其无状态化。所有文件的写入操作都应使用数据卷或者绑定宿主目录。这样写入的性能和稳定性更高。*

和虚拟机不同的是，容器中的应用都以前台执行，而并没有后台服务的概念。容器就是为了主进程存在的。主进程退出，容器就是去了存在的意义，从而退出。容器的核心是所执行的程序，所需要的资源都是应用程序所必需的。如果在伪终端中用`ps`或者`top`查看的话会发现并没有其他的进程信息。

`docker ps`可以查看所有运行的容器。如果要查看终止的容器可以使用`-a`。

### 启动容器
容器可以从镜像新建并启动，也可以从Stopped状态重新启动（使用`docker start`）。由于容器轻量的特性，很多时候容器都是随时删除而后重新创建的。

`docker run -t -i <tag> <command>`从镜像新建并启动一个容器。`-t`让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上，`-i`是交互式操作，使容器的标准输入保持打开。如果要在容器退出时删除容器则可以添加`--rm`。

当利用`docker run`来创建容器时，Docker将执行以下标准操作：
* 检查本地是否存在指定镜像。如果不存在则从公有仓库下载。
* 利用镜像创建并启动一个容器
* 分配一个文件系统并挂载到只读的镜像上层
* 从宿主机配置的网桥接口中桥接一个虚拟接口到容器中
* 从地址池配置一个IP地址给容器
* 执行用户指定的应用程序
* 执行完毕后容器被终止

#### 以后台运行
`-d`会将容器以后台运行，不将运行结果输出到当前宿主机下。容器的输出内容可以使用`docker logs`查看。

### 终止容器
可以使用`docker stop <identifier>`来终止一个运行中的容器。此外，当容器中指定的应用退出时，容器也自动终止。处于终止状态的容器可以使用`docker start <identifier>`再次启动。`docker restart <identifier>`会将一个运行中的容器终止并重新启动。

### 进入容器
很多时候我们需要进入后台运行（例如使用`-d`启动）的容器进行操作。

`docker attach <identifier>`是Docker自带的命令。当多个窗口同时attach导同一个容器时所有窗口会同步显示，并且一个窗口因故阻塞时其它窗口也不能进行操作了。

`docker exec <identifier> <command>`将在docker容器中运行指定程序。如果使用`-t -i`运行`/bin/bash`的话可以获得一个伪终端。

### 导入和导出容器
如果要导出本地的一个容器，可以使用`docker export`命令。相反，可以使用`docker import`从容器快照文件中导入镜像。注意镜像文件和容器快照文件的区别：容器快照将丢弃所有的历史纪录和元数据，仅保存容器当时的状态，体积更小。

### 删除容器
可以使用`docker rm`删除一个在终止状态的容器。如果要删除一个运行状态的容器，使用`-f`参数，Docker会发送SIGKILL到容器中。

## Docker Repository
仓库是集中存放镜像的场所。有的时候会把仓库和仓库注册服务器Registry混为一谈。一个Registry中可以包含多个仓库，每个仓库可以包含多个标签（tag），每个标签对应一个镜像。一般而言，一个仓库包含的是同一个软件不同版本的镜像，而标签则用于区分不同的版本。我们可以用<仓库名>:<标签>来指定镜像。

仓库分为公有和私有。最大的公有仓库是Docker Hub，国内的有时速云、网易云。用户可以在本地创建一个私有仓库。Docker仓库的概念和Git有些类似。

`docker search`可以搜索官方仓库中的镜像。

DockerHub支持自动创建镜像，即跟踪GitHub或BitBucket上项目。一旦项目发生新的提交则自动执行创建。

Docker官方提供`docker-registry`来构建私有的镜像仓库。

# Docker数据管理
## Data Volume数据卷
数据卷是一个可供一个或者多个容器使用的特殊目录，它绕过UFS并独立于容器和镜像之外。

### 创建数据卷
在用`docker run`的时候，使用`-v`来创建并挂载一个数据卷。在一次run中多次使用可以挂载多个数据卷。

```bash
docker run -d -P --name web -v /webapp training/webapp python app.py
```
这将创建一个数据卷并挂载到`/webapp`目录下。

`docker volume create <volume_name>`可以用来创建一个独立的卷，并随后挂载到容器中：

```bash
docker volume create my-named-volume
docker run -d -P -v my-named-volume:/webapp --name web training/webapp python app.py
```

也可以使用`VOLUME`来添加一个或者多个新的卷到Dockerfile创建的容器中。

### 挂载主机目录或文件为数据卷
```bash
docker run -d -P --name web -v /src/webapp:/webapp training/webapp python app.py
```

上述命令将主机的`/src/webapp`挂载到容器的`/webapp`。本地目录的路径必须为绝对路径，如果不存在则Docker会自动创建它。

上述指令也可以挂载单个文件。但是编辑文件时很多时候会造成文件的inode(matadata)改变而报错。简单的解决办法是挂载该文件所在目录。

Docker挂载数据卷的默认权限是读写，添加`:ro`可以指定为只读。

Dockerfile不支持这种用法。

### 挂载共享的数据卷
通过插件可以挂载各种分布式数据卷。

### 删除数据卷
数据卷的生命周期是独立于容器的，所以如果需要在删除容器时删除相应的数据卷，使用`docker rm -v`。可以使用`docker volume ls -f dangling=true`来寻找未被使用的数据卷。

### 查看数据卷信息
使用`docker inspect`可以看到容器的详细信息，其中`Volumes`部分是数据卷。所有的数据卷都是创建在主机的`/var/lib/docker/volumes/`下面的。

## 数据卷容器
数据卷容器是一个用来提供数据卷给其它容器挂载的容器。

创建数据卷容器：
```bash
docker create -v /dbdata --name dbstore training/postgres /bin/true
```

挂载数据卷到其它容器：
```bash
docker run -d --volumes-from dbstore --name db1 training/postgres
```
在一个容器中可以使用多个`--volumes-from`参数来指定多个容器挂载数据卷，也可以从其他已经挂载数据卷的容器来级联挂载：
```bash
docker run -d --name db3 --volumes-from db1 training/postgres
```

注意，作为`--volumes-from`参数的容器并不需要在运行状态。

### 利用数据卷容器来备份、恢复、迁移数据卷
1. 创建一个数据卷容器；
2. 挂载数据卷到容器1，并将数据存入数据卷；
3. 创建一个新的容器2，并将数据卷挂载到容器2；
4. 将数据卷中的数据恢复导容器2中。