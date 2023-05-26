# SLAMTEC MAPPER 激光测距传感器应用笔记

> https://bucket-download.slamtec.com/8f2e0a04ecb1e3ccca2a7a99e1349b9e0344b5ee/SQ108_SLAMTEC_MAPPER_Kit_quickstart_M1M1_v1.0_cn.pdf
>
> https://developer.slamtec.com/docs/slamware/ros-sdk/2.8.2_rtm/
>
> http://wiki.ros.org/catkin

## 下载SLAMTEC SDK

请在思岚科技官方网站上的[支持与下载页面](https://www.slamtec.com/cn/support)下载适合您的平台的ROS SDK并解压至本地。	

Slamware ROS SDK包含了您开发过程中可能会用到的资源、代码，其目录结构组织如下：

| 目录               | 说明                  |
| ------------------ | --------------------- |
| docs               | 参考文档              |
| src                | 源码                  |
| --slamware_ros_sdk | ROS SDK源码包         |
| --slamware_sdk     | SDK相关头文件与库文件 |

## 1. 配置ROS环境

1. 获取ROS Noetic Docker镜像:
` docker pull ros:noetic`

2. 运行ROS Noetic Docker容器:
` docker run -it --name ros-noetic-container ros:noetic /bin/bash

3. 安装SLAMTEC Mapper SDK: 首先，您需要从SLAMTEC官方网站下载适用于ROS的SDK（A1/A2/A3）：https://www.slamtec.com/en/Support#mapper-a-series

4. 将SDK文件上传到Docker容器中。您可以使用docker cp命令实现这一点，例如：
`docker cp slamtec_ros_sdk.tar.gz ros-noetic-container:/root`

5. 然后在Docker容器中解压缩文件：

```bash
docker exec -it ros-noetic-container bash
tar -xvzf slamtec_ros_sdk.tar.gz
```

6. 构建SLAMTEC ROS软件包: 在Docker容器内，导航到解压缩的SDK目录，然后构建ROS软件包：
```bash
cd slamtec_ros_sdk 
source /opt/ros/noetic/setup.bash 
catkin_make
```

7. 连接SLAMTEC Mapper设备： 

  SLAMTEC Mapper雷达与计算机之间是通过Wi-Fi连接,当设备正常启动后，打开您的无线网络适配器，您将看到热点 SLAMWARE-XXXXXX “ 默认 IP 为 192.168.11.1
  
  使用`--net=host`参数运行Docker容器，以便容器可以访问主机网络：
`docker run -it --name ros-noetic-container --net=host ros:noetic /bin/bash`

8. 修改SLAMTEC ROS SDK中的launch文件以使用设备的IP地址。导航到`slamtec_ros_sdk/launch`文件夹，打开`slamware.launch`文件并找到以下行：
```bash 
<arg name="ip_address" default="192.168.11.1" />
```

9. 启动SLAMTEC Mapper ROS节点

```bash
source devel/setup.bash
roslaunch slamware_ros_sdk slamware.launch
```

## 2. 启动节点

若移动机器人处于AP模式，连接机器人WIFI，启动节点

```
roslaunch slamware_ros_sdk slamware_ros_sdk_server_node.launch ip_address:=192.168.11.1
```

通过rviz查看`需要使用noetic-desktop-full镜像`

*启动容器之前执行`export DISPLAY=:0`*

```
roslaunch slamware_ros_sdk view_slamware_ros_sdk_server_node.launch
```



###3. 融合思路

1. 安装并配置ROS环境：根据您的操作系统和需要的ROS版本，安装和配置ROS环境。可以参考ROS Wiki上的教程：http://wiki.ros.org/ROS/Installation
2. 安装并配置SLAMTEC Mapper SDK：按照前面回答中的说明，安装并配置SLAMTEC Mapper SDK以从雷达设备获取数据。
3. 安装并配置摄像头驱动：为了从摄像头获取可见光图像，您需要安装和配置适当的摄像头驱动。对于许多常见的摄像头，可以使用`usb_cam`或`uvc_camera`软件包。这些软件包的详细信息和安装说明可以在ROS Wiki上找到：

- usb_cam：http://wiki.ros.org/usb_cam
- uvc_camera：http://wiki.ros.org/uvc_camera

1. 时间同步：为了将雷达数据与可见光图像对齐，您需要确保它们的时间戳是同步的。您可以使用`message_filters`库来实现这一点。有关如何使用`message_filters`的更多信息，请参阅ROS Wiki教程：http://wiki.ros.org/message_filters/Tutorials
2. 转换坐标系：为了将雷达数据与摄像头图像对齐，您需要将它们转换到相同的坐标系。这可以通过使用ROS的`tf`库实现。首先，需要定义雷达和摄像头之间的固定变换关系。然后，可以使用`tf`库将雷达数据转换到摄像头坐标系。关于`tf`库的更多信息和教程，请参阅ROS Wiki：http://wiki.ros.org/tf/Tutorials
3. 可视化数据：使用ROS的`rviz`工具可视化雷达数据和摄像头图像。这将帮助您验证数据对齐是否正确。有关如何使用`rviz`的教程，请参阅ROS Wiki：http://wiki.ros.org/rviz/Tutorials

通过以上步骤，您可以使用ROS将雷达数据与可见光数据对齐。需要注意的是，这里提供的信息仅是一个概述，实际操作时可能需要根据您的硬件和需求进行调整。