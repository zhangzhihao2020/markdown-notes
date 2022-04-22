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

## 安装aptitude后apt不能使用，aptitude换回apt，apt：找不到命令
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

## 确定Ubuntu的版本代号
在终端输入命令`lsb_release -c`
```
Ubuntu 12.04 (LTS)代号为precise。
Ubuntu 14.04 (LTS)代号为trusty。
Ubuntu 15.04 代号为vivid。
Ubuntu 15.10 代号为wily。
Ubuntu 16.04 (LTS)代号为xenial。
Ubuntu 18.04 (LTS)代号为bionic。
```


## Ubuntu18.04更换apt-install源
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
## Ubuntu18.04卸载软件
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