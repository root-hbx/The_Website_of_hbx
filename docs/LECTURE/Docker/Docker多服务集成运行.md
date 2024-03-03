>想要把多个服务同时运行，显然需要构建脚本，一键执行所有服务，并发式运行
### 编写脚本
要把项目依赖的多个服务集合到一起，我们需要编写一个`docker-compose.yml`文件，描述依赖哪些服务  

[参考文档](https://docs.docker.com/compose/)

```bash
version: "3.7"
services:
  app:
    build: ./
    ports:
      - 80:8080
    volumes:
      - ./:/app
    environment:
      - TZ=Asia/Shanghai
  redis:
    image: redis:5.0.13
    volumes:
      - redis:/data
    environment:
      - TZ=Asia/Shanghai
volumes:
  redis:
```

> ps：容器默认时间不是北京时间，增加 TZ=Asia/Shanghai 可以改为北京时间
### 一键运行
在`docker-compose.yml` 工作目录，执行：`docker-compose up`就可以跑了！

[命令参考](https://docs.docker.com/compose/reference/up/)

- 在后台运行只需要加一个 -d 参数`docker-compose up -d`  
- 查看运行状态：`docker-compose ps`  
- 停止运行：`docker-compose stop`  
- 重启：`docker-compose restart`  
- 重启单个服务：`docker-compose restart service-name`  
- 进入容器命令行：`docker-compose exec service-name sh`  
- 查看容器运行log：`docker-compose logs [service-name]`