# openrave 常见错误记载
1.没有正确加载环境，robot没有被赋值
```
Traceback (most recent call last):
  File "assignment.py", line 195, in <module>
    scene = PickPlace()
  File "assignment.py", line 52, in __init__
    self.manipulator = self.robot.SetActiveManipulator('gripper')
AttributeError: 'NoneType' object has no attribute 'SetActiveManipulator'
SoQt releasing all memory
```
解决办法
