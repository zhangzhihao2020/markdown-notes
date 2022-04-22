# Ubuntu18.04 中安装Sublime Text 3创建命令行启动和桌面快捷方式
1.Sublime Text 3下载网址 http://www.sublimetext.com/3
2.下载完成后解压得到 sublime_text_3 文件夹，将 文件夹用下面命令移至 opt 中

```
sudo mv sublime_text_3 /opt/
```
### 创建命令行启动
通过下面指令在 /usr/bin/ 下创建链接，然后可通过 sublime 命令在命令行中打开软件

```
sudo ln -s /opt/sublime_text_3/sublime_text /usr/bin/sublime 
```
### 创建桌面快捷方式，放置启动器中
1.切换到目录 /usr/share/applications/ 下

```
cd /usr/share/applications/
```
2.创建sublime text 的 Desktop Entry

```
sudo gedit sublime-text.desktop
```
3.在打开的文件编辑器中输入下面的内容，sublime_text_3 文件夹下本身自带一个 sublime_text.desktop 文件，你可以 cat 里面内容查看，根据路径修改 Exec 和 Icon 即可。
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Encoding=UTF-8
Version=1.0
Type=Application
Name=Sublime Text
GenericName=Text Editor
Comment=Sophisticated text editor for code, markup and prose
Exec=/opt/sublime_text_3/sublime_text %F
Terminal=false
MimeType=text/plain;
Icon=/opt/sublime_text_3/Icon/256x256/sublime-text.png
Categories=TextEditor;Development;
StartupNotify=true
Actions=Window;Document;

[Desktop Action Window]
Name=New Window
Exec=/opt/sublime_text/sublime_text -n
OnlyShowIn=Unity;

[Desktop Action Document]
Name=New File
Exec=/opt/sublime_text/sublime_text --command new_file
OnlyShowIn=Unity;
```
4.保存退出即可，进入文件管理器 /usr/share/applications/ 目录中
5.双击刚刚创建的 sublime-text 文件，即可打开 Sublime Text，此时应用程序面板中也多出了快捷方式，在程序坞中右键添加到收藏夹就大功告成。

### Sublime Text 3安装Package Control
1.Click the` Preferences > Browse Packages… `menu
2.Browse up a folder and then into the `Installed Packages/` folder
3.Download `Package Control.sublime-package` and copy it into the Installed Packages/ directory
4.Restart Sublime Text

1.点击`Preferences > Browse Packages`菜单
2.进入打开的目录的上层目录，然后再进入Installed Packages/目录
3.下载`Package Control.sublime-package`
https://packagecontrol.io/Package%20Control.sublime-package 并复制到`Installed Packages/`目录
4.重启Sublime Text

**使用：**
1.点击`Preferences > Package Control`或者按下`ctrl + shift + p`
2.在弹出的输入框中输入install 选择Package Control: Install Package 即将出现一大堆插件
3.输入插件名称，选中它，敲enter即可，安装成功以后重新打开sublime，
4.查看所安装的插件preferences==>package settings里面
5.卸载插件tools==》command xxx==》输入remove，然后敲enter，选中要卸载的插件，敲enter即可

5.Ubuntu sublime text3配置ctrl+鼠标左键进行函数跳转
使用ctrl+鼠标左键代替F12进行函数跳转，ctrl+鼠标右键返回函数函数调用，shift+鼠标查找该函数的引用
点击Preferences（首选项）->Browse Packages（浏览插件）进入Packages目录，然后打开User目录，查看User目录里面有没有Default.sublime-mousemap文件，如果没有则创建一个。这个文件是用来配置sublime的鼠标操作的，内容如下：

```
[
  {
    "button": "button1",
    "count": 1,
    "modifiers": ["shift"],
    "command": "goto_reference"
  },

  {
    "button": "button2",
    "count": 1,
    "modifiers": ["ctrl"],
    "command": "jump_back"
  },

  {
    "button": "button1",
    "count": 1,
    "modifiers": ["ctrl"],
    "press_command": "drag_select",
    "command": "goto_definition"
  }
]
```
其中button1代表鼠标左键，button2代表鼠标右键

https://blog.csdn.net/weixin_38533896/article/details/88976067?ops_request_misc=&request_id=&biz_id=102&utm_term=ubuntu%20sublime%20text%20%E5%A6%82%E4%BD%95%E5%9C%A8%E7%9C%8Bpython&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-88976067.first_rank_v2_pc_rank_v29

6.Sublime Text2/3怎样在Ubuntu中配置CTags插件
*打开Sublime Text 2/3软件，在Preferences(设置)菜单中打开Package Control(插件管理器)

*打开菜单后找到install packages,回车执行,拉取插件列表要等一小会

*输入ctags回车安装，稍等一会看到左下角提示安装成功就好了

*这时你在打开的文件中，右键菜单中会多一个Navigate to Definition菜单项

*这时在侧左栏的工程/项目文件上右键会看到CTags: Rebuild Tags菜单项，但是那是灰色的不可用
如果，右键菜单中执行Navigate to Definition菜单项，左下角会有如下提示：
```
Can't find any relevant tags file
```
这是因为我们还没有安装ctags

*接下来我们开始安装ctags，这个就相当简单了，执行命令
```
sudo apt-get install ctags
```
在想要建立索引文件的文件夹目录下执行：
```
sudo ctags -R *
```
发现目录下多了一个tags文件，这就是索引文件

*配置ctags路径
打开ctags的settings-default，并复制全部代码，将其粘贴到setting-user中；
找到`"command": "/home/zzh/tags",`并在以上位置加入你的ctags路径；

*这时在侧左栏的工程/项目文件上右键会看到CTags: Rebuild Tags菜单项就是可用的了
这时再选中一个函数，右键打开Navigate to Definition菜单项并执行，当然这里可以用快捷键。
这时会神奇的发现sublime text已经在一个新选项卡中打开个这个函数定义的文件选中和定位到了函数定义的地方！

https://blog.csdn.net/weixin_30538029/article/details/96987110?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161631883716780265427250%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161631883716780265427250&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-8-96987110.first_rank_v2_pc_rank_v29&utm_term=sublime+text+3%E5%9C%A8linux%E4%B8%AD%E5%AE%9E%E7%8E%B0%E8%BF%BD%E8%B8%AA%E5%87%BD%E6%95%B0%E6%88%96%E6%96%B9%E6%B3%95

https://blog.csdn.net/m0_37624499/article/details/90705658