这里是跟随[Get Started, Part 3: Services | Docker Documentation](https://docs.docker.com/get-started/part3/)
写的一些东西。

## 基础概念

**Service:** 在一个分布式的应用中，app的每一个功能碎片被称为 **services**. 例如一个视频分享网站，它包含一个用来存储应用数据的service，一个用来在用户上传视频后在后台转码的service，还有一个前端service等等。
## Your first docker-compose.yml file
```yml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: x13945/get-started:part2
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```
上面这段东西大致描述如下：
- Pull the image we uploaded in step 2 from the registry.
- Run 5 instances of that image as a service called web, limiting each one to use, at most, 10% of the CPU (across all cores), and-  50MB of RAM.
- Immediately restart containers if one fails.
- Map port 4000 on the host to web’s port 80.
- Instruct web’s containers to share port 80 via a load-balanced network called webnet. (Internally, the containers themselves-  publish to web’s port 80 at an ephemeral port.)
- Define the webnet network with the default settings (which is a-  load-balanced overlay network).

## Run your new load-balanced app
首先，在我们可以使用`docker stack deploy`之前，需要用下面的命令启动集群

```shell
docker swarm init
```
之后，我们就可以启动这个app了，同时还可以给这个app取个名字，我这里用的是`getstartedlab`：
```shell
docker stack deploy -c docker-compose.yml getstartedlab
```

现在， 我们就可以使用docker里`service`相关的命令查看service了，如：
```shell
docker service ls
```
运行在service里的container被称为 **Task**，我们可以通过`ps`命令查看这些tsk的参数：
```
docker service ps getstartedlab_web
```
## Scale the app
我们可以通过调整`docker-compose.yml`里的`replicas`参数来调整集群的个数，之后重新执行`deploy`即可生效：
```shell
docker stack deploy -c docker-compose.yml getstartedlab
```

## Take down the app and the swarm
删除分布式服务app：
```shell
docker stack rm getstartedlab
```
关闭集群
```shell
docker swarm leave --force
```
