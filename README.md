Magician Robotarm
======
本文件夹中包含了多个为Magician机械臂提供ROS支持的软件包。推荐的运行环境为 Ubuntu 16.04 + ROS Kinetic 或 Ubuntu 18.04 + ROS Melodic，其他环境下的运行情况没有测试过。

#### Ubuntu 16.04 + ROS Kinetic

**安装一些重要的依赖包**
```sh
sudo apt-get install ros-kinetic-gazebo-ros-control ros-kinetic-ros-control ros-kinetic-ros-controllers
```
**安装和升级MoveIt!** 

```sh
sudo apt-get update
sudo apt-get install ros-kinetic-moveit
```

**安装本软件包**

首先创建catkin工作空间 ([教程](http://wiki.ros.org/catkin/Tutorials))。 然后将本文件夹放到src/目录下，之后用catkin_make来编译。

#### Ubuntu 18.04 + ROS Melodic

**安装一些重要的依赖包**
```sh
sudo apt-get install ros-melodic-gazebo-ros-control ros-melodic-ros-control ros-melodic-ros-controllers
```
**安装和升级MoveIt!** 

```sh
sudo apt-get update
sudo apt-get install ros-melodic-moveit
```

**安装本软件包**

首先创建catkin工作空间 ([教程](http://wiki.ros.org/catkin/Tutorials))。 然后将本文件夹放到src/目录下，之后用catkin_make来编译。

---

### 使用仿真模型

用Gazebo仿真请运行：
```sh
roslaunch magician_description gazebo.launch
```

运行MoveIt!模块, RViz界面:
```sh
roslaunch magician_moveit_config moveit_planning_execution.launch
```

运行后台程序及Magician Control Panel界面：
```sh
roslaunch magician_background magician_background.launch
```

> 关于MoveIt!的使用方法可以参考[docs/moveit_plugin_tutorial.md](docs/moveit_plugin_tutorial.md)  
Tips:  
每次规划路径时，都要设置初始位置为当前位置。

### 使用真实机械臂
将Magician通过USB线连接到电脑。用以下命令可以查到当前电脑连接的USB设备的编号：
```sh
ls /dev/ttyUSB*
```
本软件包默认的编号是ttyUSB0 。假如当前编号不是0的话，请对magician_hardware/launch/magician_bringup.launch的相应部分进行修改。
```
  <node name="magician_hardware" pkg="magician_hardware" type="magician_hardware_node" args="ttyUSB0" output="screen"/>
```
现假设设备编号是ttyUSB0，运行以下指令来启动驱动：
```sh
sudo chmod a+rw /dev/ttyUSB0
roslaunch magician_hardware magician_bringup.launch
```
运行MoveIt!模块和RViz界面:
```sh
roslaunch magician_moveit_config moveit_planning_execution.launch
```
> 关于MoveIt!的使用方法可以参考[docs/moveit_plugin_tutorial.md](docs/moveit_plugin_tutorial.md)  
Tips:  
1.每次规划路径时，都要设置初始位置为当前位置。  
2.Magician各轴运动角度范围为 J1: [-125°~125°]; J2: [20°~90°]; J3: [35°~90°]。在此范围外时MoveIt!无法进行规划。

运行后台程序及Magician Control Panel界面：
```sh
roslaunch magician_background magician_background.launch
```


### problem solved

1. VMware: vmw_ioctl_command error Invalid argument.
   ```
   export SVGA_VGPU10=0
   ```
   or
   ```
   echo "export SVGA_VGPU10=0" >> ~/.profile
   ```