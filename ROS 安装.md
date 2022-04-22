# ROS 使用常见问题 
## ubuntu 18.04 ROS 安装
打开**终端**依次输入
```
1.sudo apt-get update -y
2.sudo apt-get upgrade -y
3.sudo apt-get install meld build-essential libfontconfig1 mesa-common-dev libglu1-mesa-dev -y
4.cd $HOME
5.sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
6.wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
7.sudo apt-get update -y
8.sudo apt-get install ros-melodic-desktop-full -y
9.sudo rosdep init
10.rosdep update
11.sudo apt-get install python-rosinstall ros-melodic-moveit ros-melodic-industrial-core python-catkin-tools ros-melodic-openni-camera ros-melodic-openni-launch ros-melodic-openni2-camera ros-melodic-openni2-launch ros-melodic-moveit-commander ros-melodic-moveit-planners ros-melodic-moveit-plugins ros-melodic-moveit-ros ros-melodic-moveit-setup-assistant ros-melodic-gazebo-ros-pkgs ros-melodic-gazebo-ros-control -y
12.echo "source /opt/ros/melodic/setup.bash" >> $HOME/.bashrc
13.source $HOME/.bashrc
14.roscore(看是否正常运行)
```
一个小测试：
终端输入
`roscore`
新终端输入，出现小乌龟窗口
`rosrun turtlesim turtlesim_node`
新终端输入，鼠标放在终端，可用方向键移动小乌龟
`rosrun turtlesim turtle_teleop_key`
新终端输入，可以看到ros图形化界面，展示节点关系
`rosrun rqt_graph rqt_graph`

##  ubuntu 16.04 ROS 安装
打开**终端**依次输入
```
	1.sudo apt-get update -y
	2.sudo apt-get upgrade -y
	3.sudo apt-get install meld build-essential libfontconfig1 mesa-common-dev libglu1-mesa-dev -y
	4.cd $HOME
	5.sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
	6.wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
	7.sudo apt-get update -y
	8.sudo apt-get install ros-kinetic-desktop-full -y
	9.sudo rosdep init
	10.rosdep update
	11.sudo apt-get install python-rosinstall ros-kinetic-moveit ros-kinetic-industrial-core python-catkin-tools ros-kinetic-openni-camera ros-kinetic-openni-launch ros-kinetic-openni2-camera ros-kinetic-openni2-launch ros-kinetic-moveit-commander ros-kinetic-moveit-planners ros-kinetic-moveit-plugins ros-kinetic-moveit-ros ros-kinetic-moveit-setup-assistant ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control -y
	12.echo "source /opt/ros/kinetic/setup.bash" >> $HOME/.bashrc
	13.source $HOME/.bashrc
	14.mkdir -p ~/5_ros/0_test/src && cd ~/5_ros/0_test/
	15.catkin build
	16.source devel/setup.bash
	17.echo $ROS_PACKAGE_PATH
```
运行完成后显示：/opt/ros/kinetic/share。

一个小测试：
终端输入，
`roscore`
新终端输入，出现小乌龟窗口，
`rosrun turtlesim turtlesim_node`
新终端输入，鼠标放在终端，可用方向键移动小乌龟，
`rosrun turtlesim turtle_teleop_key`
新终端输入，可以看到ros图形化界面，展示节点关系，
`rosrun rqt_graph rqt_graph`

### （2）安装中常见问题

1. 在`sudo rosdep init`时出现的错误:
​``` ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list ```
打开hosts文件
`sudo gedit /etc/hosts`
在文件末尾添加
`151.101.84.133 raw.githubusercontent.com`
保存后退出再尝试
2. 安装ROS时初始化rosdep过程中，执行到：
```
sodu rosdep init
```
报错： sudo: rosdep：找不到命令

原因：没有安装python-rosdep这个包
解决方法：

```
sudo apt-get install python-rosdep2
```
然后从新执行：
```
sudo rosdep init
rosdep update
```
3.输入`roscore`出现
```
Command 'roscore' not found, but can be installed with: sudo apt install python-roslaunch"score' not found, but can be installed with: sudo apt install python-roslaunch
```
解决方案
指令 `roscore`之所以能够被执行，首先需要在文件夹 “/opt/ros/indigo/bin/” 里面存在名为 `roscore`的二进制可执行文件，打开文件夹，检查文件是否存在：
```
cd /opt/ros/melodic/bin
ls -l
```
没有,输入：
```
sudo apt-get install ros-melodic-desktop
```
cd 进去再看，有了
执行
```
source /opt/ros/kinetic/setup.bash
roscore
```

---------------------------------------------------------------------------------------------------------------

1. 出现不能满足依赖关系，XX：依赖：XX 但是它将不会被安装
	解决方法：根据提示输入：`sudo apt-get -f install`
2. 运行完`sudo apt-get install ros-kinetic-desktop-full -y`时，报错出现几个软件包无法下载。
	解决方法：根据提示运行：`sudo apt-get upgate` or  `sudo apt-get install ros-kinetic-desktop-full -y --fix-missing`
3. 输入指令：`sudo apt-get install *****`
	报错：
	E: 无法获得锁 /var/lib/dpkg/lock - open (11: 资源暂时不可用)
	E: 无法锁定管理目录(/var/lib/dpkg/)，是否有其他进程正占用它？
	解决：
	`sudo rm /var/cache/apt/archives/lock  
	sudo rm /var/lib/dpkg/lock`

3. 安装好后运行：`roscore`,出现“ 程序“roscore" 尚未安装。
	解决方法：依次输入
	1.`echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc`
	2.`source ~/.bashrc`
	3.`roscore`


```??

```
