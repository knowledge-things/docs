# TensorFlow Docker安装

[Docker](https://docs.docker.com/install/) 使用容器创建虚拟环境，以便将 TensorFlow 安装结果与系统的其余部分隔离开来。TensorFlow 程序在此虚拟环境中运行，该环境能够与其主机共享资源（访问目录、使用 GPU、连接到互联网等）。我们会针对每个版本测试 [TensorFlow Docker 映像](https://hub.docker.com/r/tensorflow/tensorflow/)。

Docker 是在 Linux 上启用 TensorFlow [GPU 支持](https://www.tensorflow.org/install/gpu?hl=zh-cn)的最简单方法，因为只需在主机上安装 [NVIDIA® GPU 驱动程序](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#how-do-i-install-the-nvidia-driver)，而不必安装 NVIDIA® CUDA® 工具包。

## TensorFlow Docker 要求

1. 在本地主机上[安装 Docker](https://docs.docker.com/install/)。

2. 如需在 Linux 上启用 GPU 支持，请

   安装 NVIDIA Docker 支持

   。

   - 请通过 `docker -v` 检查 Docker 版本。对于 19.03 **之前**的版本，您需要使用 nvidia-docker2 和 `--runtime=nvidia` 标记；对于 **19.03 及之后**的版本，您将需要使用 `nvidia-container-toolkit` 软件包和 `--gpus all` 标记。这两个选项都记录在上面链接的网页上。

**注意**：如需在没有 `sudo` 的情况下运行 `docker` 命令，请创建 `docker` 组并添加您的用户。有关详情，请参阅[针对 Linux 的安装后步骤](https://docs.docker.com/install/linux/linux-postinstall/)。

## 下载 TensorFlow Docker 映像

官方 TensorFlow Docker 映像位于 [tensorflow/tensorflow](https://hub.docker.com/r/tensorflow/tensorflow/) Docker Hub 代码库中。映像版本按照以下格式进行[标记](https://hub.docker.com/r/tensorflow/tensorflow/tags/)：

| 标记        | 说明                                                         |
| :---------- | :----------------------------------------------------------- |
| `latest`    | TensorFlow CPU 二进制映像的最新版本。（默认版本）            |
| `nightly`   | TensorFlow 映像的每夜版。（不稳定）                          |
| *`version`* | 指定 TensorFlow 二进制映像的版本，例如：2.1.0                |
| `devel`     | TensorFlow `master` 开发环境的每夜版。包含 TensorFlow 源代码。 |
| `custom-op` | 用于开发 TF 自定义操作的特殊实验性映像。详见[此处](https://github.com/tensorflow/custom-op)。 |

每个基本标记都有会添加或更改功能的变体：

| 标记变体          | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| *`tag`*`-gpu`     | 支持 GPU 的指定标记版本。（[详见下文](https://www.tensorflow.org/install/docker?hl=zh-cn#gpu_support)） |
| *`tag`*`-jupyter` | 针对 Jupyter 的指定标记版本（包含 TensorFlow 教程笔记本）    |

您可以一次使用多个变体。例如，以下命令会将 TensorFlow 版本映像下载到计算机上：

```bsh
docker pull tensorflow/tensorflow                     # latest stable release
docker pull tensorflow/tensorflow:devel-gpu           # nightly dev release w/ GPU support
docker pull tensorflow/tensorflow:latest-gpu-jupyter  # latest release w/ GPU support and Jupyter
```

## 启动 TensorFlow Docker 容器

要启动配置 TensorFlow 的容器，请使用以下命令格式：

```
docker run [-it] [--rm] [-p hostPort:containerPort] tensorflow/tensorflow[:tag] [command]
```

有关详情，请参阅 [docker 运行参考文档](https://docs.docker.com/engine/reference/run/)。

### 使用仅支持 CPU 的映像的示例

我们使用带 `latest` 标记的映像验证 TensorFlow 安装效果。Docker 会在首次运行时下载新的 TensorFlow 映像：

```bsh
docker run -it --rm tensorflow/tensorflow \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

**成功**：TensorFlow 现已安装完毕。请查看[教程](https://www.tensorflow.org/tutorials?hl=zh-cn)开始使用。

我们来演示更多 TensorFlow Docker 方案。在配置 TensorFlow 的容器中启动 `bash` shell 会话：

```
docker run -it tensorflow/tensorflow bash
```

在此容器中，您可以启动 `python` 会话并导入 TensorFlow。

如需在容器内运行在主机上开发的 TensorFlow 程序，请装载主机目录并更改容器的工作目录 (`-v hostDir:containerDir -w workDir`)：

```bsh
docker run -it --rm -v $PWD:/tmp -w /tmp tensorflow/tensorflow python ./script.py
```

向主机公开在容器中创建的文件时，可能会出现权限问题。通常情况下，最好修改主机系统上的文件。

使用每夜版 TensorFlow 启动 [Jupyter 笔记本](https://jupyter.org/)服务器：

```
docker run -it -p 8888:8888 tensorflow/tensorflow:nightly-jupyter
```

按照说明在主机网络浏览器中打开以下网址：`http://127.0.0.1:8888/?token=...`

## GPU 支持

Docker 是在 GPU 上运行 TensorFlow 的最简单方法，因为主机只需安装 [NVIDIA® 驱动程序](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#how-do-i-install-the-nvidia-driver)，而不必安装 NVIDIA® CUDA® 工具包。

安装 [Nvidia 容器工具包](https://github.com/NVIDIA/nvidia-docker/blob/master/README.md#quickstart)以向 Docker 添加 NVIDIA® GPU 支持。`nvidia-container-runtime` 仅适用于 Linux。有关详情，请参阅 `nvidia-container-runtime` [平台支持常见问题解答](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#platform-support)。

检查 GPU 是否可用：

```
lspci | grep -i nvidia
```

验证 `nvidia-docker` 安装效果：

```
docker run --gpus all --rm nvidia/cuda nvidia-smi
```

**注意**：`nvidia-docker` v2 使用 `--runtime=nvidia`，而不是 `--gpus all`。`nvidia-docker` v1 使用 `nvidia-docker` 别名，而不是 `--runtime=nvidia` 或 `--gpus all` 命令行标记。

### 使用支持 GPU 的映像的示例

下载并运行支持 GPU 的 TensorFlow 映像（可能需要几分钟的时间）：

```bsh
docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

设置支持 GPU 的映像可能需要一段时间。如果重复运行基于 GPU 的脚本，您可以使用 `docker exec` 重复使用容器。

使用最新的 TensorFlow GPU 映像在容器中启动 `bash` shell 会话：

```
docker run --gpus all -it tensorflow/tensorflow:latest-gpu bash
```

**成功**：TensorFlow 现已安装完毕。请查看[教程](https://www.tensorflow.org/tutorials?hl=zh-cn)开始使用。