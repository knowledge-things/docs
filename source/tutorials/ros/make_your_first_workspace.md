# ROS创建第一个工作区

## 1. 管理你的环境

在安装ROS期间，您将看到系统提示您`获取`几个setup.*sh文件中的一个，甚至将此“sourcing”添加到您的shell启动脚本中。这是必需的

```bash
$ source /opt/ros/noetic/setup.bash
$ printenv | grep ROS
```

配置环境变量，方便在任意 终端中使用 ROS。

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## 2. Create a ROS Workspace

[catkin_make](http://wiki.ros.org/catkin/commands/catkin_make)命令是使用[catkin工作区的](http://wiki.ros.org/catkin/workspaces)便捷工具。第一次在您的工作区中运行它，它将在您的“src”文件夹中创建一个`CMakeLists.txt`链接。

此外，如果您查看当前目录，您现在应该有一个“devel”和“build”文件夹。在“devel”文件夹中，你可以看到现在有几个setup.*sh文件

```bash
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
$ source devel/setup.bash
$ echo $ROS_PACKAGE_PATH
/root/catkin_ws/src:/opt/ros/noetic/share
```

### 添加依赖包

```bash
# catkin_create_pkg 自定义ROS包名 [dependencies pkg]

$ cd src
$ catkin_create_pkg rebot-pkg rospy std_msgs
$ cd ../
$ catkin_make
```



## 3. 浏览ROS文件系统

### 1. 先决条件

```
$ sudo apt-get install ros-<distro>-ros-tutorials
```

Replace '<distro>' (including the '<>') with the name of your [ROS distribution](http://wiki.ros.org/Distributions) (e.g. indigo,noetic, kinetic, lunar etc.)

