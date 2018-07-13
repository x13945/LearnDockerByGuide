这里是跟随[Get Started, Part 2: Containers | Docker Documentation](https://docs.docker.com/get-started/part2/#apppy)写的一些东西。

## 基础概念

**Image:** 就是一个可执行的包，里面包含了应用运行所需的所有东西，如代码、运行时、类库、环境变量和配置文件等等。

**Container:** 就是Image加载到内存中运行的一个实例。

## Build the app
```shell
docker build -t friendlyhello .
```

## Run the app
```shell
docker run -p 4000:80 friendlyhello
```

## Tag the image

打标签的语法如下：
```shell
docker tag image username/repository:tag
```
那么对应我们这个例子就是：
```shell
docker tag friendlyhello x13945/get-started:part2
```

## Publish the image
发布image的docker的仓库中
```shell
docker push x13945/get-started:part2
```

## Pull and run the image from the remote repository
现在，我们就可以依照一般image的使用办法:
```shell
docker run -p 4000:80 username/repository:tag
```
使用我们刚刚发布的image了
```shell
docker run -p 4000:80 x13945/get-started:part2
```
