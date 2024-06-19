# Docker 和 ROS 指南

## 使用 ROS 创建 Docker 镜像

首先，如果可以的话，您*绝对*应该依赖[Docker Hub 上预构建的 OSRF ROS 映像](https://hub.docker.com/r/osrf/ros)

例如，要直接从源获取完整的 ROS Noetic 桌面安装：

```
docker pull osrf/ros:noetic-desktop-full
```

*Architectures*

| Type                                                 | Status                                                       |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| [amd64](https://hub.docker.com/r/amd64/ros/tags)     | [![Build Status](https://camo.githubusercontent.com/aed3cc5009ee7a0c23d4a1e8ee6954f2ca793b00c5a5726aa53f5c283ba7f13e/68747470733a2f2f646f692d6a616e6b792e696e666f73696674722e6e65742f6275696c645374617475732f69636f6e3f6a6f623d6d756c7469617263682f616d6436342f726f73)](https://doi-janky.infosiftr.net/job/multiarch/job/amd64/job/ros/) |
| [arm32v7](https://hub.docker.com/r/arm32v7/ros/tags) | [![Build Status](https://camo.githubusercontent.com/b872e8aa372ca657177f00eae5157734114d46f85438d4ab1a72b484653ed5ec/68747470733a2f2f646f692d6a616e6b792e696e666f73696674722e6e65742f6275696c645374617475732f69636f6e3f6a6f623d6d756c7469617263682f61726d333276372f726f73)](https://doi-janky.infosiftr.net/job/multiarch/job/arm32v7/job/ros/) |
| [arm64v8](https://hub.docker.com/r/arm64v8/ros/tags) | [![Build Status](https://camo.githubusercontent.com/e5fa43d6e09d48403c9005adce8f22da4c7a44766e118bbde3873b61ef765198/68747470733a2f2f646f692d6a616e6b792e696e666f73696674722e6e65742f6275696c645374617475732f69636f6e3f6a6f623d6d756c7469617263682f61726d363476382f726f73)](https://doi-janky.infosiftr.net/job/multiarch/job/arm64v8/job/ros/) |

## 启动docker容器

[Docker 网络选项](https://docs.docker.com/network/)有很多，但一种常见的选项是允许容器与主机共享同一网络。

```bash
$ docker run -it --net=host osrf/ros:noetic-desktop-full roscore
$ docker run -it --net=host osrf/ros:noetic-desktop-full bash
# rostopic list
/rosout
/rosout_agg
```

尝试在 3 个单独的终端中运行以下命令，确保通过主机网络启动 Docker 容器。

```bash
$ rscore 
 
$ rostopic pub -r 1 /my_topic std_msgs/String '{data: "hello"}'
 
$ rostopic echo /my_topic
```

## Docker 容器内的图形

现在我们应该有一个独立的 ROS 开发环境，其中包含一些 TurtleBot3 软件包……但是如果您尝试运行任何带有图形的东西，您会发现这并不能正确运行。根据[ROS Wiki](http://wiki.ros.org/docker/Tutorials/GUI)，有多种方法可以让图形在 Docker 容器内工作，这对于充满 RViz、rqt 和 Gazebo 等可视化工具的 ROS 工作流程来说至关重要。采用最简单（但最不安全）的方法，即允许 Docker 使用主机的 X11 套接字，我们可以执行以下操作：

```
export DISPLAY=:0

xhost + 

docker run -it --net=host \
    --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --volume="${PWD}":"/catkin_ws":rw \
    --name ros-noetic \
    osrf/ros:noetic-desktop-full \
    bash

```

```
docker run -it --rm --name example-ros \
		-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY \
		--net=host --privileged osrf/ros:noetic-desktop-full
```

