# ros学习笔记
## 1常用命令
 	`rqt_graph`查看通讯机制 
 	`rosnode`显示节点相关信息
 	`rosservice`
 	`rostopic`
 	`rosparam`
 	`rosmsg`
 	`rossrv`
 	`rosbag record -a -O cmd_record` 话题记录 -a(all),-O(压缩)，cmd_record（名字，可任意）
 	`rosbag play cmd_record.bag`话题复现

例：小乌龟控制小程序

```
rostopic pub -r 10  /turtle1/cmd_vel geometry_msgs/Twist "linear:
  x: 1.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.20" 
```
## 2 创建工作空间与功能包
1).工作空间(workspace)是一个存放工程开发相关文件的文件夹。
 	• src:代码空间(Source Space)
 	• build:编译空间(Build Space)
 	• devel:开发空间(Development Space)
 	• install:安装空间(Install Space)

创建工作空间
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```
编译工作空间
```
cd ~/catkin_ws/
catkin_make
```
设置和检查环境变量
```
source devel/setup.bash
echo $ROS_PACKAGE_PATH
```
生成install
```
catkin_make install
```
2).创建功能包 `catkin_create_pkg <package_name> [depend1] [depend2] [depend3]`
```
cd ~/catkin_ws/src
catkin_create_pkg test_pkg std_msgs rospy roscpp
```
编译功能包
```
cd ~/catkin_ws
catkin_make
source ~/catkin_ws/devel/setup.bash
```
**同一个工作空间下,不允许存在同名功能包,不同工作空间下,允许存在同名功能包**

## 3 发布者Publisher的编程实现
1.创建功能包
```
cd ~/catkin_ws/src
catkin_create_pkg learning_topic roscpp rospy std_msgs geometry_msgs turtlesim
```
2.在learning_topic/src创建发布者代码
3.在配置CMakeLists.txt中的编译规则（python不需要）
#########前面加入 
##Install ##
#########

```
add_executable(velocity_publisher src/velocity_publisher.cpp)
target_link_libraries(velocity_publisher ${catkin_LIBRARIES})
```
注：*velocity_publisher是代码文件名*
编译并运行发布者
```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
rosrun learning_topic velocity_publisher
```
## 4 订阅者Subscriber的编程实现
在实现publisher的基础上
1.在learning_topic/src创建订阅者代码
2.在配置CMakeLists.txt中的编译规则（python不需要）
#########前面加入 
##Install ##
#########
```
add_executable(pose_subscriber src/pose_subscriber.cpp)
target_link_libraries(pose_subscriber ${catkin_LIBRARIES})
```
3.编译并运行订阅者(python不需要编译)
```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
rosrun learning_topic pose_subscriber
```

## 5 话题消息的定义与使用
1.定义msg文件;
在功能包learning_topic里面创建msg文件夹，然后在里面创建.msg文件
```
touch Person.msg
```
然后在里面输入信息
```
string name
uint8 sex
uint8 age
uint8 unknown = 0
uint8 male= 1
uint8 female = 2
```
2.在package.xml中添加功能包依赖
```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```
3.在CMakeLists.txt添加编译选项

```
find_package( ...... message_generation)

add_message_files(FILES Person.msg)
generate_messages(DEPENDENCIES std_msgs)
################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

catkin_package(CATKIN_DEPEND ...... message_runtime)
```
4.创建发布者和订阅者代码
5.CMakeLists.txt配置代码编译规则
• 设置需要编译的代码和生成的可执行文件;
• 设置链接库;
• 添加依赖项。

```
add_executable(person_publisher src/person_publisher.cpp)
target_link_libraries(person_publisher ${catkin_LIBRARIES})
add_dependencies(person_publisher ${PROJECT_NAME}_generate_messages_cpp)

add_executable(person_subscriber src/person_subscriber.cpp)
target_link_libraries(person_subscriber ${catkin_LIBRARIES})
add_dependencies(person_subscriber ${PROJECT_NAME}_generate_messages_cpp)
```
6.编译并运行发布者和订阅者

```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun learning_topic person_subscriber
rosrun learning_topic person_publisher
```
## 6 客户端Client的编程实现
1.创建功能包
```
cd ~/catkin_ws/src
catkin_create_pkg learning_service roscpp rospy std_msgs geometry_msgs
turtlesim
```
2.在learning_service/src创建客户端代码 turtle_spawn.cpp
3.配置CMakeLists.txt中的客户端代码编译规则
```
add_executable(turtle_spawn src/turtle_spawn.cpp)
target_link_libraries(turtle_spawn ${catkin_LIBRARIES})
```
4.编译并运行客户端
```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
rosrun learning_service turtle_spawn
```
## 7 服务端Server的编程实现
1.创建功能包
```
cd ~/catkin_ws/src
catkin_create_pkg learning_service roscpp rospy std_msgs geometry_msgs
```
2.在learning_service/src创建服务端代码 turtle_command_server
3.配置CMakeLists.txt中的客户端代码编译规则
```
add_executable(turtle_command_server src/turtle_command_server.cpp)
target_link_libraries(turtle_command_server ${catkin_LIBRARIES})
```
4.编译并运行服务器
```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
rosrun learning_service turtle_command_server
rosservice call /turtle_command "{}"
```
##  服务数据的定义与使用
1.自定义服务数据
(1).定义srv文件;
在功能包learning_service里面创建srv文件夹，然后在里面创建.srv文件
```
touch Person.srv
```
然后在里面输入信息
```
string name
uint8 age
uint8 sex
uint8 unknown = 0
uint8 male = 1
uint8 female = 2
---(分隔符，前面request,后面response)
string result
```
(2).在package.xml中添加功能包依赖
```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```
(3).在CMakeLists.txt添加编译选项
```
find_package( ...... message_generation)

add_service_files(FILES Person.srv)
generate_messages(DEPENDENCIES std_msgs)
################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

catkin_package(...... message_runtime)
```
(4)编译生成语言相关文件
```
catkin_make
```
2.创建服务器代码
3.创建客户端代码
4.在CMakeLists.txt中配置服务器/客户端代码编译规则
	• 设置需要编译的代码和生成的可执行文件;
	• 设置链接库;
	• 添加依赖项.

```
add_executable(person_server src/person_server.cpp)
target_link_libraries(person_server ${catkin_LIBRARIES})
add_dependencies(person_server ${PROJECT_NAME}_gencpp)

add_executable(person_client src/person_client.cpp)
target_link_libraries(person_client ${catkin_LIBRARIES})
add_dependencies(person_client ${PROJECT_NAME}_gencpp)
```

5.编译并运行客户端和服务端

```
cd ~/catkin_ws
catkin_make
source devel/setup.bash

roscore
rosrun learning_service person_server
rosrun learning_service person_client

```
## 参数的使用与编程方法
1.创建功能包
2.参数命令行使用
```
cd ~/catkin_ws/src
catkin_create_pkg learning_parameter roscpp rospy std_srvs
```
3.在~/catkin_ws/src/config文件创建YAML参数文件
```
background_b: 255
background_g: 86
background_r: 69
rosdistro: 'melodic'
roslaunch:
  uris: {host_hcx_vpc__43763: 'http://hcx-vpc:43763/'}
rosversion: '1.14.3'
run_id: 077058de-a38b-11e9-818b-000c29d22e4d
```
4.编程
5.配置代码编译规则
```
add_executable(parameter_config src/parameter_config.cpp)
target_link_libraries(parameter_config ${catkin_LIBRARIES})
```
6.编译并运行发布者
```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
rosrun learning_parameter parameter_config
```
