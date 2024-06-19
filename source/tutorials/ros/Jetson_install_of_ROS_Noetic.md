# Jetson install of ROS Noetic

> [Ubuntu install of ROS Noetic Wiki](http://wiki.ros.org/noetic/Installation/Ubuntu)

## 1. Installation

### 1.1 Configure Ubuntu repositories配置你的仓库地址

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 1.2 设置key

```bash
sudo apt install curl # 可选如果已安装请忽略
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

### 1.3 Installation

首先更新apt仓库

```bash
sudo apt update
```

安装,建议直接step2，珍惜时间

```bash
sudo apt install ros-noetic-desktop-full
```

*大概率会失败，由于GFW问题，使用代理安装*

```bash
sudo apt install ros-noetic-desktop-full -o Acquire::http::proxy="http://192.168.55.100:7890"
```

### 1.4 Environment setup配置环境变量

#### 1.4.1临时生效，每次打开bash重新执行

```bash
source /opt/ros/noetic/setup.bash
```

#### 1.4.2永久生效

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 1.5 安装依赖包

```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

### 1.6 Initialize rosdep

```bash
sudo apt install python3-rosdep
```

```bash
sudo rosdep init
rosdep update
```

