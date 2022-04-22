# JDK安装

1.在opt文件现在jvm文件夹。

```
cd /opt
sudo mkdir jvm
```
2.将jdk安装包解压完成后移动到jvm

```
sudo rm 解压地址/jdk1.8.0_271  /opt/jvm
```
3.配置环境

```
sudo gedit ~/.bashrc
```
添加
```
export JAVA_HOME=/opt/jvm/jdk1.8.0_271
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=/usr/bin/:${JAVA_HOME}/bin:$PATH
```

保存即可。