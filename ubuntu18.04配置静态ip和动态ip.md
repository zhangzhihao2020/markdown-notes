# ubuntu18.04配置静态ip和动态ip
## 1.ubuntu18.04配置静态ip
查找netplan目录下默认的网络配置文件，文件后缀为.yaml，我的是叫01-network-manager-all.yaml的文件。如果没有可以使用sudo gedit 01-network-manager-all.yam自己创建。
``` 
cd /etc/netplan
ls
```
编辑网络配置文件之前，先查看自己的网卡名称，我的是enp0s31f6。
```
ip address
```

打开`xdg-open 01-network-manager-all.yaml`并编辑网络配置文件`01-network-manager-all.yaml`，内容如下：
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s31f6:
      dhcp4: no
      addresses: [172.20.60.193/24]
      gateway4: 172.20.60.1
      nameservers: 
        addresses: [202.202.0.33]
```
```
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s31f6:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers: 
        addresses: [192.168.1.1]
```
使用命令，使静态ip生效。
```
sudo netplan apply
```
编辑网络配置文件之前，使用ifconfig命令查看配置情况，如果配置成功上图中ip会变成自己设置的ip。
参考链接：
```
https://blog.csdn.net/u014454538/article/details/88646689?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160438439719215646549803%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=160438439719215646549803&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v28-2-88646689.first_rank_ecpm_v3_pc_rank_v2&utm_term=ubuntu1804%E9%85%8D%E7%BD%AE%E9%9D%99%E6%80%81ip%E5%92%8C%E5%8A%A8%E6%80%81IP&spm=1018.2118.3001.4449
```
2.ubuntu18.04配置动态ip
查看网卡名称，参考上文。
查找网络配置文件，参考上文。
修改网络配置文件的内容如下： 
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
     enp3s0: #配置的网卡名称,使用ifconfig -a查看得到
       dhcp4: true #dhcp4开启
       addresses: [] #设置本机IP及掩码，空
       optional: true
```
使用`sudo netplan apply`命令，使动态生效。之后再使用`ifconfig`命令查看配置情况，如果配置成功上图中ip会变成动态的ip.