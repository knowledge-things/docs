# Docker Pull 设置代理

## 问题现象

如果不配置代理服务器就直接拉镜像，docker 会直接尝试连接镜像仓库，并且连接超时报错。如下所示：

```shell
$ docker pull busybox
Using default tag: latest
Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled 
while waiting for connection (Client.Timeout exceeded while awaiting headers)
```

## 容易误导的官方文档

有这么一篇关于 docker 配置代理服务器的 [官方文档](https://docs.docker.com/network/proxy/#configure-the-docker-client) ，如果病急乱投医，直接按照这篇文章配置，是不能成功拉取镜像的。

我们来理解一下这篇文档，文档关键的原文摘录如下：

> If your container needs to use an HTTP, HTTPS, or FTP proxy server, you can configure it in different ways: Configure the Docker client On the Docker client, create or edit the file ~/.docker/config.json in the home directory of the user that starts containers.
>
> …
>
> When you create or start new containers, the environment variables are set automatically within the container.

这篇文档说：如果你的容器需要使用代理服务器，那么可以以如下方式配置： 在运行容器的用户 home 目录下，配置 `~/.docker/config.json` 文件。重新启动容器后，这些环境变量将自动设置进容器，从而容器内的进程可以使用代理服务。

所以这篇文章是讲如何配置运行容器的环境，与如何拉取镜像无关。如果按照这篇文档的指导，如同南辕北辙。

要解决问题，我们首先来看一般情况下命令行如何使用代理。

## 环境变量

常规的命令行程序如果要使用代理，需要设置两个环境变量：`HTTP_PROXY` 和 `HTTPS_PROXY` ，设置环境变量的方法见 [这篇文章](https://www.lfhacks.com/test/cypress-download-failure#env) 。但是仅仅这样设置环境变量，也不能让 docker 成功拉取镜像。

我们仔细观察 [上面的报错信息](https://www.lfhacks.com/tech/pull-docker-images-behind-proxy/#problem)，有一句说明了报错的来源：

> Error response from daemon:

因为镜像的拉取和管理都是 docker daemon 的职责，所以我们要让 docker daemon 知道代理服务器的存在。而 docker daemon 是由 systemd 管理的，所以我们要从 systemd 配置入手。

## 正确的官方文档

关于 systemd 配置代理服务器的 [官方文档在这里](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)，原文说：

> The Docker daemon uses the HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environmental variables in its start-up environment to configure HTTP or HTTPS proxy behavior. You cannot configure these environment variables using the daemon.json file.
>
> This example overrides the default docker.service file.
>
> If you are behind an HTTP or HTTPS proxy server, for example in corporate settings, you need to add this configuration in the Docker systemd service file.

这段话的意思是，docker daemon 使用 `HTTP_PROXY`, `HTTPS_PROXY`, 和 `NO_PROXY` 三个环境变量配置代理服务器，但是你需要在 systemd 的文件里配置环境变量，而不能配置在 `daemon.json` 里。

## 具体操作

下面是来自 [官方文档](https://www.lfhacks.com/tech/pull-docker-images-behind-proxy/#correct) 的操作步骤和详细解释：

1. 创建 dockerd 相关的 systemd 目录，这个目录下的配置将覆盖 dockerd 的默认配置

```shell
$ sudo mkdir -p /etc/systemd/system/docker.service.d
```

1. 新建配置文件 `/etc/systemd/system/docker.service.d/http-proxy.conf`，这个文件中将包含环境变量

```ini
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80"
Environment="HTTPS_PROXY=https://proxy.example.com:443"
```

1. 如果你自己建了私有的镜像仓库，需要 dockerd 绕过代理服务器直连，那么配置 `NO_PROXY` 变量：

```ini
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80"
Environment="HTTPS_PROXY=https://proxy.example.com:443"
Environment="NO_PROXY=your-registry.com,10.10.10.10,*.example.com"
```

多个 `NO_PROXY` 变量的值用逗号分隔，而且可以使用通配符（*），极端情况下，如果 `NO_PROXY=*`，那么所有请求都将不通过代理服务器。

1. 重新加载配置文件，重启 dockerd

```shell
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

1. 检查确认环境变量已经正确配置：

```shell
$ sudo systemctl show --property=Environment docker
```

1. 从 `docker info` 的结果中查看配置项。