# 飞控 & PX4

## 仿真环境介绍

Ubuntu 20.04

ROS Noetic

PX4 1.14.0

## 依赖安装

```
sudo apt install ninja-build exiftool ninja-build protobuf-compiler libeigen3-dev genromfs xmlstarlet libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gawk
```

## 安装python3

用 apt 安装 python

```
sudo apt install python3 python3-pip
```

安装 rospkg

```
pip3 install rospkg
```

安装依赖

```
pip3 install --user toml empy jinja2 packaging
```

## 安装PX4

克隆仓库

```
git clone https://github.com/PX4/PX4-Autopilot.git --branch v1.14.0 --recursive
```

因为px4仓库里面放了很多其他的仓库，这步在网络不稳定时极容易失败，最好能一次全部通过，若有部分仓库报错未克隆完全，可以用下面的指令再次进行子模块更新

```
cd PX4-Autopilot
git submodule update --init --recursive
```

若一直子模块更新失败也可以使用下面的网盘下载好解压到home目录后直接执行下面的编译操作

[网盘下载链接](https://www.123912.com/s/CVqajv-amTCd)

```sh
cd Tools/setup
source ubuntu.sh
```

编译

```
cd ../..
make px4_sitl_default gazebo-classic_iris
```

编译完成会有如下文字

<font color="green">[Msg] Waiting for master.</font>

<font color="green">[Msg] Connected to gazebo master @ http://127.0.0.1:11345</font>

<font color="green">[Msg] Publicized address: 192.168.1.113</font>

出现 gazebo，里面会有**一架飞机**，则说明编译成功，按`ctrl+c`关闭进程等待gazebo自动关闭

然后将下面的内容写入`~/.bashrc`文件的最后

```bash
source ~/PX4-Autopilot/Tools/simulation/gazebo-classic/setup_gazebo.bash ~/PX4-Autopilot ~/PX4-Autopilot/build/px4_sitl_default
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic
```

> **重新**打开一个终端会出现几个路径，则设置代表成功
>

!!! note
    如果不想每次都看见那几行路径，则去`setup_gazebo.bash`中把echo的几句话全部注释掉即可
    若你后续增加了新的工作空间，一定要把这三行放在最后在 souce，之前的工作空间按创建时间顺序 souce

## 测试 PX4

**新开**一个终端，运行下面的launch

!!! note
    一定要新开一个终端！！

```bash
roslaunch px4 mavros_posix_sitl.launch
```

运行`rostopic echo /mavros/state`   其中`connected`为 true 则成功

```bash
$ rostopic echo /mavros/state
header: 
  seq: 29
  stamp: 
    secs: 29
    nsecs: 532000000
  frame_id: ''
connected: True
armed: False
guided: True
manual_input: False
mode: "AUTO.LOITER"
system_status: 3
```
