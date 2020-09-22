# Docker

![docker](https://tozii.github.io/Asset/document/images/docker.png)

&emsp;&emsp;Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口

[官网](https://www.docker.com/)

[Docker官方容器库](https://hub.docker.com/)

[阿里云容器库](https://cr.console.aliyun.com)

[Docker官方文档](https://docs.docker.com/)

[Docker中文文档](https://yeasy.gitbooks.io/docker_practice/content/introduction/)

## Docker基本操作命令

[Docker 常用命令 — 平飞](https://www.cnblogs.com/zyrmb/p/10509524.html)

**一、拉取镜像**
`docker pull imageName`

说明：<br/>
1、imageName：镜像名称

**二、构建镜像（使用Dockerfile构建镜像）**
`docker build -t imageName .`

说明：

1、imageName：镜像名称

**三、查看镜像**
`docker images`

**四、运行容器（运行容器，并且在后台运行，暴露端口80）**

`docker run --name containerName -d -p 80:80 -v {/data/}:{/user/publish/} imageName`

说明：

1、containerName：容器名称

2、-d 保持后台进程运行

3、-p 端口映射，{主机端口}:{容器端口}

4、-v 目录映射，{主机路径}:{容器路径}

5、imageName：镜像名称

**五、查看容器**
`docker ps -a  // (列出所有容器，包括未运行的)`

`docker ps -a -q  // (列出所有容器ID)`

**六、删除镜像**
`docker rmi imageName`

说明：

1、imageName：镜像名称或者镜像Id

**七、删除所有镜像**
`docker rmi $(docker images -q)`

**八、删除容器**
`docker rm containerName`

`docker rm $(docker ps -a -q)   //  删除所有容器`

说明：

1、containerName：容器名称

**九、 删除所有容器**
`docker rm $(docker ps -a -q)`

**十、 查看容器日志**
`docker logs -f -t --since=“2019-03-04” --tail 10 containerName`

说明：

1、-f 输出日志

2、-t 显示时间戳

3、--since=“2019-03-04” 显示该时间后的日志

4、--tail 10 列出容器最新10条日志

5、containerName 容器名称

**十一、 查看容器配置**
`docker inspect containerName`

**十二、停止容器**
`docker stop containerName`

**十三、保存镜像**
`docker save -o name.tar imageName`

说明：

1、name.tar 需要保存的文件名称

2、imageName 镜像名称

**十四、加载本地镜像**
`docker load -I name.tar`

说明：

1、name.tar 需要加载的文件名称

**十五、导出容器**
`docker export -o containerName.tar containerName`
说明：

1、containerName.tar 需要保存的容器文件名称

2、containnerName 需要导出的容器名称

**十六、导入容器**
`docker import containerName.tar containerName`

说明：

1、containerName.tar 需要导入的容器文件名称

2、containnerName 需要导入的容器名称

**十七、启动容器（针对已经停止运行的容器）**
`docker start containerName`

**十八、重启容器**
`docker restart containerName`

**十九、进入容器**
> Linux

`docker exec -it containerName bash`

> Windows

`docker exec -it containerName cmd`

**二十、容器与主机之间的数据拷贝**

> 将主机./RS-MapReduce目录拷贝到容器30026605dcfe的/home/cloudera目录下。

`docker cp RS-MapReduce 30026605dcfe:/home/cloudera`

> 将容器30026605dcfe的/home/cloudera/RS-MapReduce目录拷贝到主机的/tmp目录中。

`docker cp  30026605dcfe:/home/cloudera/RS-MapReduce /tmp/`