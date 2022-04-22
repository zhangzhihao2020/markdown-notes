# Pycharm 使用常见问题
## 1. Pycharm 安装
1.输入下面命令

​	`mkdir -p ~/9_software`
​	`cd ~/9_software && sudo tar -xzf pycharm-professional-2019.1.3.tar.gz`
​	`~/9_software/pycharm-2019.1.3/bin/pycharm.sh`
2.出现Complete-Installation提示框，如果需要导入之前安装版本的配置的话，就选第一个，没有就选第二个。
<div align=center  ><img src="/home/zzh/桌面/markdown笔记/图片/20180718022736394.png"  width="60%"  height="60%"  >


所以这里选第二个，直接点OK，如图3.弹出 PyCharm Privary Policy Agreement，隐私政策协议框，点击Accept 
4.选择 jetbrains account,账户密码如下：
	guofuyu@cqu.edu.cn
	Loser327416
5.安装完成
<div align=center  ><img src="/home/zzh/桌面/markdown笔记/图片/20180718040831393.png"  width="60%"  height="60%"  >


6.创建快捷方式
Ubuntu的快捷方式都放在/usr/share/applications，首先在该目录下创建一个Pycharm.desktop 
启用root权限，新打开一个终端，键入sudo -i 
输入密码即可 
再键入: `sudo gedit /usr/share/applications/Pycharm.desktop` 
然后在打开的文档中输入以下内容，注意Exec和Icon需要找到正确的路径 ，然后再到/usr/share/applications中找到相应的启动，进入后锁定到启动器即可。

```[Desktop Entry]
Version=1.0
Type=Application
Name=PyCharm
Icon=/opt/pycharm-pycharm-2019.1.3/bin/pycharm.png    #注意此处的路径
Exec="/opt/pycharm-pycharm-2019.1.3/bin/pycharm.sh" %f   #注意此处的路径
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false Startup
WMClass=jetbrains-pycharm
```
