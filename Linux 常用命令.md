# Linux 常用命令
## 1.处理目录的常用命令
ls（英文全拼：list files）: 列出目录及文件名
```
ls 目录名称
ls -al ~ (将home目录下的所有文件列出来(含属性与隐藏档))
```
cd（英文全拼：change directory）：切换目录
```
cd 相对路径或绝对路径 (使用路径切换)
cd ~  (回到自己的家目录)
cd .. (回到上一级目录)
```
pwd（英文全拼：print work directory）：显示目前的目录
```
pwd
pwd -P  (-P ：显示出确实的路径，而非使用连结 (link) 路径)
```
mkdir（英文全拼：make directory）：创建一个新的目录
```
mkdir -mp 目录名称 (-m ：配置文件的权限;-p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来)
```
rmdir（英文全拼：remove directory）：删除一个空的目录
```
rmdir -p 目录名称 (-p ：连同上一级『空的』目录也一起删除)
```
cp（英文全拼：copy file）: 复制文件或目录
```
cp 目录/原文件 目录/新文件
    -a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
    -d：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
    -f：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
    -i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
    -l：进行硬式连结(hard link)的连结档创建，而非复制文件本身；
    -p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
    -r：递归持续复制，用於目录的复制行为；(常用)
    -s：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
    -u：若 destination 比 source 旧才升级 destination ！
```
rm（英文全拼：remove）: 移除文件或目录
```
rm -fir 文件或目录
    -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
    -i ：互动模式，在删除前会询问使用者是否动作
    -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！
```
mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称
```
mv 目录/原文件 目录/新文件
    -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
    -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
    -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
```


apt 常用命令

    列出所有可更新的软件清单命令：sudo apt update
    
    升级软件包：sudo apt upgrade
    
    列出可更新的软件包及版本信息：apt list --upgradeable
    
    升级软件包，升级前先删除需要更新软件包：sudo apt full-upgrade
    
    安装指定的软件命令：sudo apt install <package_name>
    
    安装多个软件包：sudo apt install <package_1> <package_2> <package_3>
    
    更新指定的软件命令：sudo apt update <package_name>
    
    显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：sudo apt show <package_name>
    
    删除软件包命令：sudo apt remove <package_name>
    
    清理不再使用的依赖和库文件: sudo apt autoremove
    
    移除软件包及配置文件: sudo apt purge <package_name>
    
    查找软件包命令： sudo apt search <keyword>
    
    列出所有已安装的包：apt list --installed
    
    列出所有已安装的包的版本信息：apt list --all-versions

