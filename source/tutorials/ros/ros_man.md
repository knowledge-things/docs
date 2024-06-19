# ROS常用命令

 ## 1. Filesystem Tools


### 1.1 rospack

[rospack](http://wiki.ros.org/rospack) allows you to get information about packages

Usage: 

```bash
$ rospack find [package_name]
```

Example: 

```bash
$ rospack find roscpp
```

###1.2 roscd

roscd是 [rosbash]( http://wiki.ros.org/rosbash)套件的一部分

Usage: 

```
$ roscd <package-or-stack>[/subdir]
```

Example: 

```bash
$ roscd roscpp
```

#### 1.2.1 Subdirectories

```
$ roscd roscpp/cmake
```

### 1.3 roscd log

```
$ roscd log
```

### 1.4 rosls

Usage: 

```bash
$ rosls <package-or-stack>[/subdir]
```

Example: 

```bash
$ rosls roscpp_tutorials
```

## 2. Creating a catkin Package

### 2.1 catkin_create_pkg

You should have created this in the Creating a Workspace Tutorial
`$ cd ~/catkin_ws/src`

[catkin_create_pkg](http://wiki.ros.org/catkin/commands/catkin_create_pkg) 

Usage:

```bash
catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
```

Example: 

```bash
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

## 3. Building a catkin workspace 

Now you need to build the packages in the catkin workspace:

For more advanced uses of [catkin_make](http://wiki.ros.org/catkin/commands/catkin_make) see the documentation: [catkin/commands/catkin_make](http://wiki.ros.org/catkin/commands/catkin_make)

Usage: 

```bash
# In a catkin workspace
$ catkin_make [make_targets] [-DCMAKE_VARIABLES=...]
```

Example: 

```bash
$ cd ~/catkin_ws
$ catkin_make
```

To add the workspace to your ROS environment you need to source the generated setup file: 

```bash
$ . ~/catkin_ws/devel/setup.bash
```

## 4. package dependencies

Usage:

```bash
$ rospack depends1 [package_name]
```

Example: 

```bash
$ rospack depends1 roscpp 
```

如果需要显示包之间的递归关系使用`rospack depends`,这个命令列出了指定包的所有依赖，包括直接和间接依赖。

```bash
$ rospack depends [package_name]
```

## 5. ROS Nodes

### 5.1 roscore

`roscore` is the first thing you should run when using ROS. 

Please run: 

```bash
$ roscore
```

If `roscore` does not initialize, you probably have a network configuration issue. See [Network Setup - Single Machine Configuration](http://www.ros.org/wiki/ROS/NetworkSetup#Single_machine_configuration)

### 5.2 rosnode

rosnode is a command-line tool for printing information about ROS Nodes.

```
Commands:
        rosnode ping    test connectivity to node
        rosnode list    list active nodes
        rosnode info    print information about node
        rosnode machine list nodes running on a particular machine or list machines
        rosnode kill    kill a running node
        rosnode cleanup purge registration information of unreachable nodes

Type rosnode <command> -h for more detailed usage, e.g. 'rosnode ping -h'
```

The `rosnode list` command lists these active nodes:

```
$ rosnode list
```

The `rosnode info` command returns information about a specific node.

```bash
$ rosnode info /rosout
```

### 5.3 rosrun

Usage: 

```
$ rosrun [package_name] [node_name]
```

So now we can run the turtlesim_node in the turtlesim package. 

Then, in a **new terminal**: 

```
$ rosrun turtlesim turtlesim_node
```

## 6. ROS Topics

rostopic is a command-line tool for printing information about ROS Topics.

```
Commands:
        rostopic bw     display bandwidth used by topic
        rostopic delay  display delay of topic from timestamp in header
        rostopic echo   print messages to screen
        rostopic find   find topics by type
        rostopic hz     display publishing rate of topic    
        rostopic info   print information about active topic
        rostopic list   list active topics
        rostopic pub    publish data to topic
        rostopic type   print topic or field type

Type rostopic <command> -h for more detailed usage, e.g. 'rostopic echo -h'
```

### 6.1 Using rqt_graph

`rqt_graph` creates a dynamic graph of what's going on in the system

```bash
$ rosrun rqt_graph
```

### 6.2 Using rostopic echo

Usage: 

```bash
rostopic echo [topic]
```

Let's look at the command velocity data published by the `turtle_teleop_key` node. 

*For ROS Hydro and later,* this data is published on the `/turtle1/cmd_vel` topic. **In a new terminal, run:**

```
$ rostopic echo /turtle1/cmd_vel
```

### 6.3 Using rostopic type

Usage: 

```bash
rostopic type [topic]
```

Try: 

```bash
$ rostopic type /turtle1/cmd_vel
```

### 6.3 Using rostopic pub

Usage: 

```
rostopic pub [topic] [msg_type] [args]
```

example: 

```bash
$ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
```

### 6.4 Using rosmsg

We can look at the details of the message using `rosmsg`:

```
$ rosmsg show geometry_msgs/Twist
```

```
  geometry_msgs/Vector3 linear
    float64 x
    float64 y
    float64 z
  geometry_msgs/Vector3 angular
    float64 x
    float64 y
    float64 z
```

