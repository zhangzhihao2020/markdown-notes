# Ubuntu 使用常见问题
##  1.Ubuntu + windows 双系统下Ubuntu访问windows磁盘文件方法
直接打开磁盘时出现下面错误

![](/home/zhangzhihao/桌面/markdown笔记/图片/20180531165436263.png)

这时候注意观察上图提示信息：Error mounting /dev/sda3
然后打开终端，输入指令：`sudo ntfsfix /dev/sda3`

![](/home/zhangzhihao/桌面/markdown笔记/图片/20180531170014197.png)



运行成功后就可以打开磁盘了。

## 2.Ubuntu 解决bash ./没有那个文件或目录的方法
打开终端运行
	`sudo apt-get install ia32-libs`
如果运行失败，根据提示，重新运行
	`sudo apt-get install lib32z1`

## 3.Ubuntu 屏幕截取命令
1、捕捉整个屏幕
`gnome-screenshot`
2、通过 -w 参数来捕捉当前 Shell 窗口
`gnome-screenshot -w`
3、使用 -a 参数来捕捉指定区域
`gnome-screenshot -a`
4、使用 -b 参数来去除窗口的边框
 `nome-screenshot -w -b`
5、使用 -d 参数来延迟截取功能从而截取其他活动窗口
 `nome-screenshot -d 5`
6、使用 -e 参数来给截图添加效果
注：我使用 -w 参数来捕捉当前窗口，-b 参数来去除窗口名称状态条，同时给它添加一个边框：
  `gnome-screenshot -wb -e border`

## 4. 安装aptitude后apt不能使用，aptitude换回apt，apt：找不到命令
首先卸掉aptitude
```
sudo dpkg -r aptitude
```
*一般到此即可*
到https://www.ubuntuupdates.org/搜索`apt` 和 `libapt-pkg `并下载版本`apt_1.7.0_amd64.deb `和 `libapt-pkg5.0_1.8.0_amd64.deb`
进http://archive.ubuntu.com/ubuntu/pool/main/u/ubuntu-keyring/下载`ubuntu-keyring_2018.02.28_all.deb`
均下载到主目录。之后依次执行
```
sudo dpkg -i ubuntu-keyring_2018.02.28_all.deb
sudo dpkg -i libapt-pkg5.0_1.8.0_amd64.deb
sudo dpkg -i apt_1.7.0_amd64.deb
```
验证是否可行
```
apt-get moo
```

```
                 (__) 
                 (oo) 
           /------\/ 
          / |    ||   
         *  /\---/\ 
            ~~   ~~   
..."Have you mooed today?"...
```

## 5.确定Ubuntu的版本代号
在终端输入命令`lsb_release -c`
```
Ubuntu 12.04 (LTS)代号为precise。
Ubuntu 14.04 (LTS)代号为trusty。
Ubuntu 15.04 代号为vivid。
Ubuntu 15.10 代号为wily。
Ubuntu 16.04 (LTS)代号为xenial。
Ubuntu 18.04 (LTS)代号为bionic。
```


## 6.Ubuntu18.04更换apt-install源
首先进入/etc/apt/文件夹，命令`cd /etc/apt/`
然后备份sources.list文件，防止修改错了。命令`sudo cp sources.list sources.list.backup`
再编辑sources.list文件，命令`sudo gedit sources.list`（如果你会vi/vim也可以用它，只要能编辑就行）

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```
保存后，直接关掉编辑窗口，回到终端执行命令`sudo apt-get update`，更新获取 阿里软件源 提供的软件列表。再执行命令`sudo apt-get upgrade`，更新apt-get工具。
## 7.Ubuntu18.04卸载软件
1.打开一个终端，输入`sudo dpkg --list `,按下Enter键，终端输出以下内容，显示的是你电脑上安装的所有软件。
2.在终端中找到你需要卸载的软件的名称，列表是按照首字母排序的。
3.在终端上输入命令`sudo apt-get --purge remove 包名`（--purge是可选项，写上这个属性是将软件及其配置文件一并删除，如不需要删除配置文件，可执行`sudo apt-get remove 包名`） ，如此处我要删除的是polipo ，那么在终端输入`sudo apt-get --purge remove polipo`，按下回车，输入密码，再次回车。
4..执行过程中，会提示你是否真的要删除（继续执行删除命令），在终端输入y ，然后回车，删除程序继续执行。
5.正常情况下，再次出现输入命令行删除成功。

## linux下修改hosts文件没有权限
```
cd /etc
sudo gedit hosts 
```

## 8.ubuntu18一直紫屏，无法进入图形界面
#### 首先进入想办法进入桌面环境

第一种方法
进入引导界面，选择恢复选项：https://blog.csdn.net/davidhopper/article/details/79156410
1.进入操作系统选择界面
2.选择`*ubuntu 高级选项`
3.选取最新Linux内核的修复模式项（末尾带`recovery mode`），按“Enter”键进入： 
第二种方法
进入引导界面，按 e 键；
找到 splash，按空格，加上nomodeset，按Ctrl+X启动即可。

#### 然后修改一些配置文件

```
sudo gedit /boot/grub/grub.cfg
#所有的
linux /boot/…略… ro quiet splash $vt_handoff
#改为
linux /boot/…略… ro quiet splash $vt_handoff acpi_osi=linux nomodeset
```
```？
sudo gedit /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT=“quiet splash”
#改为
GRUB_CMDLINE_LINUX_DEFAULT=“quiet splash acpi_osi=linux nomodeset”
```

## 9.Ubuntu 双屏显示设置方法

查看当前连接屏幕信息

```
xrandr
```

显示如下

```
Screen 0: minimum 8 x 8, current 3760 x 1920, maximum 16384 x 16384
DP-0 connected 1200x1920+2560+0 right (normal left inverted right x axis y axis) 488mm x 297mm
   1920x1200     59.95*+  74.94  
   1920x1080     60.00    59.94    50.00    60.00    50.04  
   1680x1050     59.95  
   1440x900      59.89  
   1280x1024     75.02    60.02  
   1280x960      60.00  
   1280x720      60.00    59.94    50.00  
   1024x768      75.03    70.07    60.00  
   800x600       75.00    72.19    60.32    56.25  
   720x576       50.00  
   720x480       59.94  
   640x480       75.00    72.81    59.93    59.94  
DP-1 disconnected (normal left inverted right x axis y axis)
DP-2 disconnected (normal left inverted right x axis y axis)
DP-3 disconnected (normal left inverted right x axis y axis)
DP-4 disconnected (normal left inverted right x axis y axis)
DP-5 connected primary 2560x1440+0+0 (normal left inverted right x axis y axis) 699mm x 393mm
   2560x1440     59.95*+
   2880x1620     60.00  
   2048x1152     60.00  
   1920x1200     59.88  
   1920x1080     60.00    59.94    50.00    29.97    25.00    23.98  
   1680x1050     59.95  
   1600x1200     60.00  
   1600x900      60.00  
   1280x1024     75.02    60.02  
   1280x720      60.00    60.00    59.94    50.00  
   1024x768      75.03    70.07    60.00  
   800x600       75.00    72.19    60.32    56.25  
   720x576       50.00  
   720x480       59.94  
   640x480       75.00    72.81    59.93    59.94  
DP-6 disconnected (normal left inverted right x axis y axis)
DP-7 disconnected (normal left inverted right x axis y axis)
```

我这里主屏为DP-4，外接屏为DP-1

- 复制屏幕

```
xrandr --output DP-4  --same-as DP-1 --auto
```

- 设置主副屏

```
xrandr --output DP-4 --primary
```

- 右扩展屏幕

```
xrandr --output DP-1  --right-of DP-4 --auto
```

左扩展屏幕

```
xrandr --output DP-0  --left-of DP-5 --auto
```

只显示副屏

```
xrandr --output DP-0 --auto --output DP-5 --off 
```

- 外接屏左转

```
xrandr --output DP-1 --rotate right
```

```
xrandr -o left 向左旋转90度
xrandr -o right 向右旋转90度
xrandr -o inverted 上下翻转
xrandr -o normal 回到正常角度
```

## 10.Ubuntu 下上传代码到Github

第一步：建立git仓库。

```
git init
```

第二步：将项目的所有文件添加到仓库中

```
git add . #所有
git add filename #单个文件
```

这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。
如果想添加某个特定的文件，只需把.换成特定的文件名即可

第三步：将add的文件commit到仓库

```
git config --global user.email "2970541981@qq.com"
git config --global user.name "zhangzhihao2020"
git commit -m "注释"
```

第四步：

```
git remote add origin git@github.com:tangqian-cqu/zzh-robot.git
```

第五步，上传代码到github远程仓库

```
git push -u origin master
```

执行完后，如果没有异常，等待执行完就上传成功了，中间可能会让你输入Username和Password，你只要输入github的账号和密码就行了.

第一次上传有可能会遇到push失败的情况，那是因为跟SVN一样，github上有一个README.md 文件没有下载下来 。我们得先

```
git pull --rebase origin master
```

然后执行：       

```
git push -u origin master
```

参考网址https://www.jianshu.com/p/e61c4ceec96c

## 11.Ubuntu18.04 LTS 升级Python3.8

#### 1、下载源码

从官网找到Python3.8最新版本文件：Python源码获取，博主当时最新版本为3.8.6，然后选择Python-3.8.x.tgz的文件下载，可以直接点击网站下载，也可以复制连接，在命令行下使用wget下载：

```
wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz1
```

解压源码并进入目录，解压文件名根据实际下载版本填写：

```
tar -zxvf Python-3.8.6.tgz
cd Python-3.8.6
```

#### 2、准备编译

首先需要配置安装路径：

```
./configure --with-ssl --prefix=/usr/local/python3
```

接着更新系统并安装相关依赖：

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install build-essential python-dev python-setuptools python-pip python-smbus libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev libssl-dev openssl libffi-dev
```

#### 3、编译并安装

编译：

```
make
```

编译完成后，使用下述命令安装：

```
sudo make install
```

由于系统原来默认的Python版本不是3.8，所以需要进行相关配置：

先删除原来的软链接：

````
sudo rm -rf /usr/bin/python3
sudo rm -rf /usr/bin/pip3
````


接着新建软链接：

```
sudo ln -s /usr/local/python3/bin/python3.8 /usr/bin/python3
sudo ln -s /usr/local/python3/bin/pip3.8 /usr/bin/pip3
```


至此，Python3.8升级完成，在命令行输入Python3，出现下述界面，说明升级成功：

```
Python 3.8.6 (default, Jul 28 2020, 12:59:40) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information
```

————————————————
版权声明：本文为CSDN博主「SameWorld」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/baidu_26678247/article/details/109731510
