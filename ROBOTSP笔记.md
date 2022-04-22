# ROBOTSP笔记
#### 网址
[urdf](https://github.com/crigroup/or_urdf "urdf")
https://github.com/crigroup/or_urdf
https://crigroup.github.io/robotsp/

#### 配置工作空间环境
```
sudo gedit ~/.bashrc
```
文件末尾添加下面代码，保存
```
source ~/catkin_ws/devel/setup.bash
```
#### python包安装
出现`no module named xxx `,说明缺乏xxx包，需要安装。
常见包安装
```
pip install xxx==1.1.1 --user
```
私有包安装，先下载，然后运行setup.py
```
sudo python setup.py install --user
```
####  When running `nosetests -v`, the following error occurs:

AttributeError: 'Graph' object has no attribute 'nodes_iter'
运行下面解决

```
pip install networkx==1.9.1 --user
```
#### Linux 下删除 /usr/local 下的文件和目录（删除python包）
依次运行下面命令
```
#进入root下
sudo su
cd /home/zzh/.local/lib/python2.7/site-packages
/usr/local/lib/python2.7/dist-packages/
/home/zzh/.local/lib/python2.7/site-packages/robotsp/solver.py

#查看所有包 
ls
#删除mxnet-1.1.0-py2.7.egg包
rm -rf mxnet-1.1.0-py2.7.egg
```
#### python编译出现`SyntaxError: Non-ASCII character ‘\xe8’ in file`
出现这个问题主要是编译中出现了中文或特殊字符，所以可以使用以下方式解决：
在文件头部加上（一定要加在第一行）
```
# -*- coding: utf-8 -*-
```
或者
```
# coding:utf-8 
```
#### .xml文件路径地址问题报错
```
2021-03-14 16:25:36,351 openrave [WARN] [libopenrave.cpp:799 __cxx11::string OpenRAVE::RaveGlobal::FindLocalFile] could not find file "/worlds/airbus_challenge.env.xml"
2021-03-14 16:25:36,351 openrave [WARN] [environment-core.h:501 Load] load failed on file /worlds/airbus_challenge.env.xml
[ERROR] [rtsp_challenge]: Failed to load: /worlds/airbus_challenge.env.xml
Traceback (most recent call last):
  File "airbus_challenge_example.py", line 50, in <module>
    raise IOError
IOError
```
解决方法：
找到`/worlds/airbus_challenge.env.xml`在代码中位置，根据airbus_challenge.env.xml文件真正的位置对路径进行修改
####  .xml文件里面调用文件路径地址错误
```
[INFO] [rtsp_challenge]: Loading OpenRAVE environment: /home/zzh/catkin_ws/src/raveutils/data/worlds/airbus_challenge.env.xml
2021-03-14 16:50:14,155 openrave [WARN] [libopenrave.cpp:799 __cxx11::string OpenRAVE::RaveGlobal::FindLocalFile] could not find file "objects/driller_base.kinbody.xml"
2021-03-14 16:50:14,155 openrave [ERROR] [xmlreaders-core.cpp:545 ParseXMLFile] xmlSAXUserParseFile: error parsing /home/zzh/catkin_ws/src/raveutils/data/worlds/airbus_challenge.env.xml: openrave (Failed): kinbody name: "" is not valid
2021-03-14 16:50:14,155 openrave [WARN] [environment-core.h:501 Load] load failed on file /home/zzh/catkin_ws/src/raveutils/data/worlds/airbus_challenge.env.xml
[ERROR] [rtsp_challenge]: Failed to load: /home/zzh/catkin_ws/src/raveutils/data/worlds/airbus_challenge.env.xml
Traceback (most recent call last):
```
解决方法：
打开对应xml文件，查看`file="objects/airbus_jig.kinbody.xml"`路径是否有问题，进行修改。

