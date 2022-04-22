# ubuntu下给软件配置桌面快捷方式
终端输入
```
sudo gedit /usr/share/applications/Qv2ray.desktop（根据自己软件名修改）
```
在打开页面输入，保存
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Exec=/home/zzh/Downloads/科学上网/Qv2ray.v2.6.3.linux-x64.AppImage（软件启动方式路径）
Name=Qv2ray（软件名）
Comment=Qv2ray（软件名）
Icon=/home/zzh/Downloads/科学上网/Qv2ray.jpg（快捷方式图标路径）
```
终端输入
```
sudo chmod +x /usr/share/applications/Qv2ray.desktop
```