### 学习目标
把前面的 Web 项目增加一个 Redis 依赖，多跑一个 Redis 容器，演示如何多容器之间的通信
### 过程展示
#### 1. 创建虚拟网络
多容器之间互通，从 Web 容器访问 Redis 容器，我们只需要把他们放到同个网络中就可以了。
[文档参考](https://docs.docker.com/engine/reference/commandline/network/)
#### 2. 网络互联使用公有容器
##### - 创建一个名为`test-net`的网络：
`docker network create test-net`
##### - 运行 Redis 在 `test-net` 网络中，别名`redis`：
`docker run -d --name redis --network test-net --network-alias redis redis:latest`
##### - 修改代码中访问`redis`的地址为网络别名：
![image.png](https://cos.easydoc.net/46901064/files/kv98rfvb.png)
##### - 运行 Web 项目，使用同个网络：
`docker run -p 8080:8080 --name test -v D:/test:/app --network test-net -d test:v1`
##### - 查看数据：
`http://localhost:8080/redis`  
容器终端查看数据是否一致