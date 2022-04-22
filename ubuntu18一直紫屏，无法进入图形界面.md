# ubuntu18一直紫屏，无法进入图形界面
## 首先进入想办法进入桌面环境
第一种方法
进入引导界面，选择恢复选项：https://blog.csdn.net/davidhopper/article/details/79156410
1.进入操作系统选择界面
2.选择`*ubuntu 高级选项`
3.选取最新Linux内核的修复模式项（末尾带`recovery mode`），按“Enter”键进入： 
第二种方法
进入引导界面，按 e 键；
找到 splash，按空格，加上nomodeset，按Ctrl+X启动即可。
## 然后修改一些配置文件
```
sudo gedit /boot/grub/grub.cfg
#所有的
linux /boot/…略… ro quiet splash $vt_handoff
#改为
linux /boot/…略… ro quiet splash $vt_handoff acpi_osi=linux nomodeset
```
```
sudo gedit /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT=“quiet splash”
#改为
GRUB_CMDLINE_LINUX_DEFAULT=“quiet splash acpi_osi=linux nomodeset”
```