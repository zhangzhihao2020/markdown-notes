# openrave 学习笔记

## 1.Python中循环后使用list.append()数据被覆盖问题的解决
```
list=[]
dictionary={} #放在循环外
keylist = table.row_values(0, 0, ncols)
for rownum in range(1,nrows):
    for colnum in range(ncols):
        dictionary[keylist[colnum]] = table.cell(rownum, colnum).value
    # print dictionary
    list.append(dictionary)
    print list
print list
```
上面获得的数据是{A1，A1}类型的数据

修改代码（正确代码）
```
list=[]
keylist = table.row_values(0, 0, ncols)
for rownum in range(1,nrows):
    dictionary = {}#放在循环内
    for colnum in range(ncols):
        dictionary[keylist[colnum]] = table.cell(rownum, colnum).value
    # print dictionary
    list.append(dictionary)
    print list
print list
```
得到数据{A1，A2}

原理分析：
dict（字典）赋给list的是一个位置，对于第一种代码，dictionary定义在循环外，每次使用list.append(dictionary)赋给list的都是相同的位置，而在同一位置的dict的值已经改变了，所以list取到的之前位置的值改变了，表现出后面数据覆盖前面数据的表象。dict定义在循环内，相当于每一次循环生成一个dictionary，占用不同的位置存储值，所以可以赋给list不同元素不同的位置，获得不同的值。
深层次解释：
Python中的对象之间赋值时是按引用传递的，如果需要拷贝对象，需要使用标准库中的copy模块。
1. copy.copy 浅拷贝 只拷贝父对象，不会拷贝对象的内部的子对象。 
2. copy.deepcopy 深拷贝 拷贝对象及其子对象

## 2. 标记空间点
```
handles = []
handles.append(robot.GetEnv().plot3( points=np.array(Tgrasp[:3,3]), pointsize=0.01, colors=np.array( (1,0,0) ), drawstyle=1) )
```
## 3. 打印
```
raveLogInfo("Robot "+robot.GetName()+" has "+repr(robot.GetDOF())+" joints with values:\n"+repr(robot.GetDOFValues()))
```
robot.GetDOF()：关节数
robot.GetDOFValues()：关节角度

## 4. set joint value 
set joint x to value y
```
robot.SetDOFValues([y],[x])
```
set all joint value
```
robot.SetActiveDOFValues([0.1, 0.7, 1.5, -0.5, -0.8, -1.2])
```
得到末端位置
```
manipulator.GetEndEffectorTransform()
```

## 5. get the transform of link x
```
T = robot.GetLinks()[x].GetTransform()
```
## 6. matrix From AxisAngle
```
T = matrixFromAxisAngle([x,y,z])
```
x,y,z代表绕x,y,z的旋转角度

## 7. Using a BiRRT Planner
使用Planner得到一个无碰撞的路径到位形空间的目标。
Use a planner to get a collision free path to a configuration space goal.

```
from openravepy import *
env = Environment() # create the environment
env.SetViewer('qtcoin') # start the viewer
env.Load('data/lab1.env.xml') # load a scene
robot = env.GetRobots()[0] # get the first robot
RaveSetDebugLevel(DebugLevel.Debug) # set output level to debug

manipprob = interfaces.BaseManipulation(robot) # create the interface for basic manipulation programs
manipprob.MoveManipulator(goal=[-0.75,1.24,-0.064,2.33,-1.16,-1.548,1.19]) # call motion planner with goal joint angles
robot.WaitForController(0)
```
## 8. Get collision-free solution
```
sol = manip.FindIKSolution(Tgoal, IkFilterOptions.CheckEnvCollisions)
```
## 9. 得到环境内物体的名称
```
env = Environment()
env.GetBodies() #得到环境中所有物体名称
body=env.GetKinBody('物体名称')
T=target.GetTransform()#得到物体位姿
ab=body.ComputeAABB().extents()#得到物体大小
```

## 10. Grabbing Object with Planner
演示如何使用Planner来使用计划关闭和打开抓手。
Shows how to use a planner to close and open a gripper using planning.

```
from openravepy import *
import numpy
env = Environment() # create openrave environment
env.SetViewer('qtcoin') # attach viewer (optional)
env.Load('data/lab1.env.xml') # load a simple scene

robot=env.GetRobots()[0]
manip = robot.GetActiveManipulator()
ikmodel = databases.inversekinematics.InverseKinematicsModel(robot,iktype=IkParameterization.Type.Transform6D)
if not ikmodel.load():
    ikmodel.autogenerate()

manipprob = interfaces.BaseManipulation(robot) # create the interface for basic manipulation programs
Tgoal = numpy.array([[0,-1,0,-0.23],[-1,0,0,-0.1446],[0,0,-1,0.85],[0,0,0,1]])
res = manipprob.MoveToHandPosition(matrices=[Tgoal],seedik=10) # call motion planner with goal joint angles
robot.WaitForController(0) # wait

taskprob = interfaces.TaskManipulation(robot) # create the interface for task manipulation programs
taskprob.CloseFingers() # close fingers until collision
# taskprob.ReleaseFingers() # 打开手指
robot.WaitForController(0) # wait
with env:
    robot.Grab(env.GetKinBody('mug4'))  #抓住物体'mug4'
   #robot.Release(env.GetKinBody('mug4')) #松开物体'mug4'
   # move manipulator to all zeros, set jitter to 0.04 since cup is initially colliding with table/移动机械手
    manipprob.MoveManipulator(numpy.zeros(len(manip.GetArmIndices())),jitter=0.1)
robot.WaitForController(0) # wait
```
## 11. Testing a Grasp
装载抓取模型，将机器人移动到第一个发现的抓取处。
Loads the grasping model and moves the robot to the first grasp found.

```
from openravepy import *
import numpy, time
env=Environment()
env.Load('data/lab1.env.xml')
env.SetViewer('qtcoin')
robot = env.GetRobots()[0]
target = env.GetKinBody('mug1') #抓取目标
gmodel = databases.grasping.GraspingModel(robot,target)
if not gmodel.load():
    gmodel.autogenerate()

validgrasps, validindicees = gmodel.computeValidGrasps(returnnum=1)
gmodel.moveToPreshape(validgrasps[0])
Tgoal = gmodel.getGlobalGraspTransform(validgrasps[0],collisionfree=True)
# 将机械手移到抓取目标处
basemanip = interfaces.BaseManipulation(robot)
basemanip.MoveToHandPosition(matrices=[Tgoal])
robot.WaitForController(0)
#合上机械爪
taskmanip = interfaces.TaskManipulation(robot)
taskmanip.CloseFingers()
robot.WaitForController(0)
```

## 12. Returning a Trajectory

在执行之前先得到一个轨迹(生成轨迹，执行，把杯子移动到目标位置)
```
from openravepy import *
import numpy, time
env=Environment()
env.Load('data/lab1.env.xml')
env.SetViewer('qtcoin')
robot = env.GetRobots()[0]
target = env.GetKinBody('mug1')
manip = robot.GetActiveManipulator()

# 生成轨迹
gmodel = databases.grasping.GraspingModel(robot,target)
if not gmodel.load():
    gmodel.autogenerate()
validgrasps, validindicees = gmodel.computeValidGrasps(returnnum=1)
basemanip = interfaces.BaseManipulation(robot) #为基本操作程序创建接口
with robot:
    grasp = validgrasps[0]
    gmodel.setPreshape(grasp)
    T = gmodel.getGlobalGraspTransform(grasp,collisionfree=True)
    traj = basemanip.MoveToHandPosition(matrices=[T],execute=False,outputtrajobj=True)
    # basemanip.MoveToHandPosition(matrices=[T],execute=False,outputtrajobj=True)，matrices=‘位姿矩阵’
    #traj=manipprob.MoveManipulator(goal=goal,execute=False,outputtrajobj=True)，goal=‘每个关节角度’
# 打印轨迹数目（traj.GetNumWaypoints()）和最后一条轨迹（repr(traj.GetWaypoint(-1)))）
raveLogInfo('traj has %d waypoints, last waypoint is: %s'%(traj.GetNumWaypoints(),repr(traj.GetWaypoint(-1))))
robot.GetController().SetPath(traj)#执行
robot.WaitForController(0)


#合上手指
taskmanip = interfaces.TaskManipulation(robot) #为任务操作程序创建接口
taskmanip.CloseFingers()
robot.WaitForController(0)

with env:
	robot.Grab(env.GetKinBody('mug1')) 
	basemanip.MoveManipulator(numpy.zeros(len(manip.GetArmIndices())),jitter=0.1)
robot.WaitForController(0) # wait

taskmanip.ReleaseFingers()
robot.Release(env.GetKinBody('mug1'))

target1 = env.GetKinBody('mug4')

gmodel = databases.grasping.GraspingModel(robot,target1)
if not gmodel.load():
    gmodel.autogenerate()
validgrasps, validindicees = gmodel.computeValidGrasps(returnnum=1)
basemanip = interfaces.BaseManipulation(robot)
with robot:
    grasp1 = validgrasps[0]
    gmodel.setPreshape(grasp1)
    T1= gmodel.getGlobalGraspTransform(grasp1,collisionfree=True)
    traj1 = basemanip.MoveToHandPosition(matrices=[T1],execute=False,outputtrajobj=True)
robot.GetController().SetPath(traj1)
robot.WaitForController(0)
```
## 13. Sampling Trajectory
获取执行轨迹，将轨迹按照时间间隔获取轨迹点的位置，然后按照时间间隔和轨迹的移动机械臂。
```
from openravepy import *
from numpy import arange
import time
env = Environment() # create the environment
env.SetViewer('qtcoin') # start the viewer
env.Load('data/lab1.env.xml') # load a scene
robot = env.GetRobots()[0] # get the first robot

#获取轨迹
manipprob = interfaces.BaseManipulation(robot) # create the interface for basic manipulation programs
traj=manipprob.MoveManipulator(goal=[-0.75,1.24,-0.064,2.33,-1.16,-1.548,1.19],outputtrajobj=True,execute=False) # call motion planner with goal joint angles

spec=traj.GetConfigurationSpecification() # get the configuration specification of the trajrectory
for i in range(5):
    starttime = time.time()
    while time.time()-starttime < traj.GetDuration():
        curtime = time.time()-starttime
        with env: # have to lock environment since accessing robot
            #获得本次for循环中，轨迹中距starttime间隔 curtime时间的点
            trajdata=traj.Sample(curtime)
            values=spec.ExtractJointValues(trajdata,robot,range(robot.GetDOF()),0)
            robot.SetDOFValues(values)
        time.sleep(0.01)
```

## 同时获得box索引、和值得方法

```
for index, cur_box in reversed(list(enumerate(self.boxes))):
```

enumerate在字典上是枚举、列举的意思,对于一个可迭代的（iterable）/可遍历的对象（如列表、字符串），enumerate将其组成一个索引序列，利用它可以同时获得索引和值enumerate多用于在for循环中得到计数。
reserved() 是 Pyton 内置函数之一，其功能是对于给定的序列（包括列表、元组、字符串以及 range(n) 区间），该函数可以返回一个逆序序列的迭代器（用于遍历该逆序序列）。