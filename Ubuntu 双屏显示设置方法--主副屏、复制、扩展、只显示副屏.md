# Ubuntu 双屏显示设置方法
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

