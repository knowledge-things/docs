在Docker容器中设置时区通常是通过在容器创建或启动时设置环境变量或者在容器内修改相关配置文件来完成的。下面是两种常见的方法来设置容器的时区：

1. **通过环境变量设置时区**: 当你使用`docker run`命令来启动一个容器时，你可以通过设置环境变量`TZ`来指定时区。对于上海，你应该使用`Asia/Shanghai`作为时区名称。例如：

   ```sh
   docker run -e TZ=Asia/Shanghai ...
   ```

2. **在容器内修改配置文件**: 如果你已经有一个正在运行的容器，你可以进入容器然后手动更改时区。首先，使用`docker exec`进入容器：

   ```sh
   docker exec -it <container_id_or_name> /bin/bash
   ```

   然后，使用以下命令来设置时区：

   ```sh
   ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   echo "Asia/Shanghai" > /etc/timezone
   ```

   注意，这个方法会直接修改容器内的配置，如果你重新创建容器，你需要再次执行这些步骤。

如果你正在使用Dockerfile来创建自己的镜像，你也可以在Dockerfile中使用上述命令来设置时区，例如：

```Dockerfile
FROM your-base-image

# 设置时区为上海
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
```

然后通过构建和运行此Dockerfile来创建并启动包含特定时区设置的容器。