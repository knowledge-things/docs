# Docker ChatGLM-6B

在往期的搭建深度学习的博客中，我们利用`Docker`搭建了`NVIDIA+PyTorch`深度学习容器，现在将利用搭建好的深度学习容器中运行以目前比较火热的`ChatGLM-6B`清华开源的自然语言AI模型。

## 搭建步骤

### 编写`DockerFile`文件

```
# 设置基础映像
FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

# 定义构建参数
# 例如ARG USER=test为USER变量设置默认值"test"。
ARG USER=test
ARG PASSWORD=${USER}123$

# 处理讨厌的 Python 3 编码问题
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV MPLLOCALFREETYPE 1

# 更新软件包列表并安装软件属性通用包
RUN apt-get update && apt-get install -y software-properties-common

# 添加Python ppa，以便后续安装Python版本
RUN add-apt-repository ppa:deadsnakes/ppa

# 安装Ubuntu的常用软件包，包括wget、vim、curl、zip、unzip等
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    wget \
    vim \
    curl \
    zip \
    zlib1g-dev \
    unzip \
    pkg-config \
    libgl-dev \
    libblas-dev \
    liblapack-dev \
    python3-tk \
    python3-wheel \
    graphviz \
    libhdf5-dev \
    python3.9 \
    python3.9-dev \
    python3.9-distutils \
    openssh-server \
    swig \
    apt-transport-https \
    lsb-release \
    libpng-dev \
    ca-certificates &&\
    apt-get clean &&\
    ln -s /usr/bin/python3.9 /usr/local/bin/python &&\
    ln -s /usr/bin/python3.9 /usr/local/bin/python3 &&\
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py &&\
    python3 get-pip.py &&\
    rm get-pip.py &&\
    # 清理APT缓存以减小Docker镜像大小
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 设置时区
ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 为应用程序创建一个用户
RUN useradd --create-home --shell /bin/bash --groups sudo ${USER}
RUN echo ${USER}:${PASSWORD} | chpasswd
USER ${USER}
ENV HOME /home/${USER}
WORKDIR $HOME

# 安装一些Python库，例如numpy、matplotlib、scipy等
RUN python3 -m pip --no-cache-dir install \
    blackcellmagic\
    pytest \
    pytest-cov \
    numpy \
    matplotlib \
    scipy \
    pandas \
    jupyter \
    scikit-learn \
    scikit-image \
    seaborn \
    graphviz \
    gpustat \
    h5py \
    gitpython \
    ptvsd \
    Pillow==6.1.0 \
    opencv-python

# 安装PyTorch和DataJoint等其他库
# 其中torch==1.13.1表示安装版本为1.13.1的PyTorch
RUN python3 -m pip --no-cache-dir install \
    torch==1.13.1 \
    torchvision==0.14.1 \
    torchaudio==0.13.1 \
    'jupyterlab>=2'

RUN python3 -m pip --no-cache-dir install datajoint==0.12.9

# 设置环境变量LD_LIBRARY_PATH，以便支持性能分析库
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:${LD_LIBRARY_PATH}

# 启动ssh服务器，打开22号端口
USER root

RUN mkdir /var/run/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
```

### 第一步：准备Docker容器&AI模型文件

- 修改`Docker Compose`文件，添加`volumes`路径

```
version: "3.9"
services:
  nvidia:
    build: .        # 告诉Docker Compose在当前目录中查找Dockerfile并构建镜像
    runtime: nvidia # 启用nvidia-container-runtime作为Docker容器的参数，从而实现对GPU的支持
    environment:
      - NVIDIA_VISIBLE_DEVICES=all  # 设置所有可用的GPU设备
    ports:
      - "22:22"         # port for ssh
      - "80:80"         # port for Web
      - "8000:8000"     # port for API
    tty: true           # 创建一个伪终端以保持容器运行状态
    # 添加一个和宿主机连接的路径
    volumes:
      - ./:/data
```

- 下载对应的AI模块文件

从`Hugging Face Hub`下载所需要的模型，由于我使用的显卡只有`8G`显存，所以直接下载了`INT4`量化后的模型。

> **AI模型下载地址：**
>
> - [THUDM/chatglm-6b](https://huggingface.co/THUDM/chatglm-6b)
> - [THUDM/chatglm-6b-int4](https://huggingface.co/THUDM/chatglm-6b-int4)
> - [THUDM/chatglm-6b-int8](https://huggingface.co/THUDM/chatglm-6b-int8)

这里推荐使用`Git`工具进行拖拽对应的仓库，在拖拽前记得给`Git`工具安装上[Git LFS](https://docs.github.com/zh/repositories/working-with-files/managing-large-files/installing-git-large-file-storage)。

仓库存储的地方就放在我当前创建`Docker Compose`文件目录下，这样刚好可以利用`volumes`将其映射进容器中。

```
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://huggingface.co/THUDM/chatglm-6b-int4

# if you want to clone without large files – just their pointers
# prepend your git clone with the following env var:
GIT_LFS_SKIP_SMUDGE=1
```

### 第二步：准备ChatGLM-6B并运行

- 拉取官方`ChatGLM-6B`项目仓库文件

```
git clone https://github.com/THUDM/ChatGLM-6B.git
```

仓库存储的地方依旧是当前创建`Docker Compose`文件目录。如果大家不希望存放在该目录下可以自行修改一下`Docker Compose`中的`volumes`映射路径。

- 拉起该深度学习`Docker`容器

```
docker-compose up --build -d
```

- 进入容器中

```
# 查看刚刚启动的深度学习容器的ID号
docker ps

# 进入容器内部
docker exec -it xxxxxxx /bin/bash # xxxxxxx 是PS后容器的CONTAINER ID号
```

- 安装项目依赖

```
# cd到刚刚拖拽下来的项目仓库中

cd /data/ChatGLM-6B

# 安装项目依赖文件

pip install -r requirements.txt
```

- 修改本地AI模型路径

在这里我们使用官方提供的命令行Demo程序来做测试。

```
# 打开cli_demo.py文件对其进行修改
VI cli_demo.py

# 修改第6、第7行的代码，将原有的模型名称修改为本地AI模型路径
# 修改结果如下,其中/data/chatglm-6b-int4为你本地AI模型的路径地址
tokenizer = AutoTokenizer.from_pretrained("/data/chatglm-6b-int4", trust_remote_code=True)
model = AutoModel.from_pretrained("/data/chatglm-6b-int4", trust_remote_code=True).half().cuda()
```

- 运行仓库中命令行Demo程序：

```
python cli_demo.py
```