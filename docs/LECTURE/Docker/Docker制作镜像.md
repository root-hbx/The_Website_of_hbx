forked from：[某不知名Web项目](https://github.com/gzyunke/test-docker)  
- 简介：这是一个 Nodejs + Koa2 写的 Web 项目，提供了简单的两个演示页面  
- 软件依赖：[nodejs](https://nodejs.org/zh-cn/)  
- 项目依赖库：koa、log4js、koa-router
### 编写 Dockerfile
1. 具体的写法得看个人配置
2. 命令参考：[Dockerfile文档](https://docs.docker.com/engine/reference/builder/#run)

> Tips：  
> 如果你写 Dockerfile 时经常遇到一些运行错误，依赖错误等，你可以==直接运行一个依赖的底==，然后进入终端进行配置环境，成功后再把做过的步骤命令写道 Dockerfile 文件中，这样编写调试会快很多  
> 
> 例如上面的底是`node:11`，我们可以运行`docker run -it -d node:11 bash`，跑起来后进入容器终端配置依赖的软件，然后尝试跑起来自己的软件，最后把所有做过的步骤写入到 Dockerfile 就好了
### Build 为镜像（安装包）和运行
1. 编译 `docker build -t test:v1 .`
> `-t` 设置镜像名字和版本号  
> 命令参考：[https://docs.docker.com/engine/reference/commandline/build/](https://docs.docker.com/engine/reference/commandline/build/)

2. 运行 `docker run -p 8080:8080 --name test-hello test:v1`
> `-p` 映射容器内端口到宿主机  
> `--name` 容器名字  
> `-d` 后台运行  
> 命令参考文档：[https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)
### 常见命令
- `docker ps` 查看当前运行中的容器  
- `docker images` 查看镜像列表  
- `docker rm container-id` 删除指定 id 的容器  
- `docker stop/start container-id` 停止/启动指定 id 的容器  
- `docker rmi image-id` 删除指定 id 的镜像  
- `docker volume ls` 查看 volume 列表  
- `docker network ls` 查看网络列表