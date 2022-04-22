##　Ubuntu18.04 LTS 升级Python3.8

###　1、下载源码

从官网找到Python3.8最新版本文件：Python源码获取，博主当时最新版本为3.8.6，然后选择Python-3.8.x.tgz的文件下载，可以直接点击网站下载，也可以复制连接，在命令行下使用wget下载：

```
wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz1
```

解压源码并进入目录，解压文件名根据实际下载版本填写：

```
tar -zxvf Python-3.8.6.tgz
cd Python-3.8.6
```


### 2、准备编译
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


### 3、编译并安装
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