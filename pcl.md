## Ubuntu18.04安装PCL1.9.1

#### Tip:

ros本身带有pcl,如果安装了完整版的ros,其实可以直接用pcl.但我在用时出现了一些问题，因此重新装了pcl1.9.1

#### 01 安装依赖库

将以下**内容保存为install_pcl_dependences.sh** ，使用在ubuntu 命令行终端输入`sudo sh install_pcl_dependences.sh` 即可进行安装，在下载安装依赖库过程中会提示是否安装，都输入y

```
sudo apt-get update
sudo apt-get install git build-essential linux-libc-dev
sudo apt-get install cmake cmake-gui 
sudo apt-get install libusb-1.0-0-dev libusb-dev libudev-dev
sudo apt-get install mpi-default-dev openmpi-bin openmpi-common
sudo apt-get install libpcap-dev
sudo apt-get install libflann1.9 libflann-dev
sudo apt-get install libeigen3-dev
sudo apt-get install libboost-all-dev
sudo apt-get install vtk6 libvtk6.3 libvtk6-dev libvtk6.3-qt libvtk6-qt-dev 
sudo apt-get install libqhull* libgtest-dev
sudo apt-get install freeglut3-dev pkg-config
sudo apt-get install libxmu-dev libxi-dev 
sudo apt-get install mono-complete
sudo apt-get install libopenni-dev libopenni2-dev
```

 

#### 02 下载源码

```
git clone https://github.com/PointCloudLibrary/pcl.git 
```

#### 03 编译安装(大约半个小时，耐心等待)

```
cd pcl
# 切换到指定版本v1.9.1再编译
git checkout pcl-1.9.1
# 创建目录
mkdir release
# 进入目录
cd release
# 配置cmake
cmake -DCMAKE_BUILD_TYPE=None \
      -DCMAKE_INSTALL_PREFIX=/usr/local \
      -DBUILD_GPU=ON \
      -DBUILD_apps=ON \
      -DBUILD_examples=ON ..
# 进行编译  ,也可以`make -j11`11为内核数 按自己的cpu内核填写 不写数字默认使用全部核心编译
make 
```

#### 04 安装

```
sudo make install
```

#### 05 测试

找一个点云pcd文件查看,输入以下代码查看

test文件中有PCD文件

```
pcl_viewer ../test/pcl_logo.pcd
```

测试是否成功，打开窗口看到logo点云即为成功安装



## 解决：

#### 1.使用 CUDA 10.1 (PTX ISA v6.4) 编译 PCL 失败

改代码！！！

https://github.com/scottmudge/pcl/commit/9e24e98ddc87d2dc62915fbdb8f966882d81f793

#### 2.fatal error: cuda_runtime.h: No such file or directory

首先 ，查看你的~/.bashrc 文件 里面有没有

export PATH="/usr/local/cuda-9.0/bin:$PATH" 
export LD_LIBRARY_PATH="/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH" 

其次看看你的makefile文件是否对cuda的地址是/usr/local/cuda-9.0 有的默认是/usr/local/cuda，所以要ln -s /usr/local/cuda-9.0 /usr/local/cuda

如果以上情况都不是，并且locate  cuda_runtime.h 能找到正确位置

那么试试这个命令吧

sudo apt-get install nvidia-cuda-toolkit

#### 3.linux下无法打开包括文件：“pcl/io/pcd_io.h”: No such file or directory

`gedit .bashrc`

添加：

`export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/include/pcl1.8:/usr/include/eigen
#加eigen的原因是调用的时候发现还缺少eigen库`

`source .bashrc`

#### 4.ubuntu怎么卸载pcl

`sudo rm -r /usr/include/pcl-1.9 /usr/share/pcl-1.9 /usr/bin/pcl* /usr/lib/libpcl* #移除与pcl相关文件`

#### 5.fatal error: pcl/surface/on_nurbs/fitting_surface_tdm.h: 没有那个文件或目录 

 `sudo cp -r ./on_nurbs /usr/local/include/pcl-1.9/pcl/surface/`

将/home/hcq/pcl/pcl/surface/include/pcl/surface/on_nurbs 移动到/usr/local/include/pcl-1.9/pcl/surface/

#### 6.报错 error: ‘shared_ptr’ is not a member of ‘pcl’

```pcl::shared_ptr ```改为``` boost::shared_ptr```

