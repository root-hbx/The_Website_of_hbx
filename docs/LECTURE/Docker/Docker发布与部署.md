### 镜像仓库介绍
镜像仓库用来存储我们 build 出来的“安装包”，Docker 官方提供了一个 [镜像库](https://hub.docker.com/)，里面包含了大量iso

我们也可以把自己 build 出来的镜像上传到 docker 提供的镜像库中，方便传播，当然你也可以搭建自己的私有镜像库，或者使用国内各种大厂提供的镜像托管服务，例如：阿里云、腾讯云
### 自行上传 iso
- 首先 [注册一个账号](https://hub.docker.com/)
- 创建一个镜像库  
- 命令行登录账号：`docker login -u username`
- 新建一个tag，名字必须跟你注册账号一样`docker tag test:v1 username/test:v1`
- 推上去 `docker push username/test:v1`
- 部署 `docker run -dp 8080:8080 username/test:v1`