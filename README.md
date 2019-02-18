
# 1 OLDX-开源多旋翼开发平台项目  
<div align=center><img width="600" height="130" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/logo.JPG"/></div>
<div align=center><img width="440" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/fc.jpg"/></div>
  OLDX多旋翼开发平台（OLDX-FC）是由北京理工大学自动化学院所属《北理云逸科技》团队开发的一个目前国内最完整的免费开源飞控项目，随着国内开源飞控的逐步发展如匿名、
INF、无名和ACFly飞控的陆续推出，如光流、气压计和GPS等相关算法已经逐步完善，但是相比Pixhawk等国外开源飞控平台的发展和定位仍然有发展空间。OLDX-FC于14年开始对多旋翼飞行器进行研究期间也经历过开源和借鉴的过程，为希望进一步推动国内开源飞控协作开发和
相互学习、相互分享的趋势，团队将该OLDX-FC转化为开源项目，采用自由捐赠的形式继续发展[捐赠地址](https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/pay.png)
。
项目遵循GPL协议，能自由下载项目PCB进行加工使用但请勿作为商业用途，开源所有飞行控制和组合导航源码，可以进行修改和二次开发。<br><br><br>

**项目荣誉**

项目|奖项|年份
-------------|-------------|-------------
IMAV国际微小型无人机大赛|室外赛第3名  室内最佳自动化|2018
中航工业杯|三等奖|2018
IMAV国际微小型无人机大赛|室外赛第5名|2017
中航工业杯|二等奖|2017
中航工业杯|三等奖|2016

**-如果该项目对您有帮助请 Star 我们的项目-**<br>
**-如果您愿意分享对该项目的优化和改进请联系golaced@163.com或加入我们的QQ群567423074，加速开源项目的进度-**<br>

	
# 2 基本功能介绍
该项目是一个辅助飞控开发的虚拟仿真环境，其基于ROS和Gazebo实现对四旋翼飞行器的快速仿真，项目基于Ardrone提供的SDK建立运行模块和基本
的姿态控制、高度控制和位置控制，另外对原始程序进行了修改可以自定义前置和下置相机的角度。通过ROS框架能订阅实时图像信息并开展对于图像
导航和视觉避障的研究，另外在计算资源足够的前提下可以添加任意多架飞行器并进行集群飞行的仿真。该项目于OLDX-FC可无缝链接，该项目运行的控制
节点产生的控制能直接实现对飞行器的控制，因此可以实现从仿真到实践的无缝移植，项目已经基于该理念实现在仿真环境中的动平台识别、位姿估计和
降落并直接在ODroid-Xu4嵌入式处理器上快速部署。
 
 
# 3 软件安装与环境配置
## 3.1 Linux配置
推荐使用Vmware虚拟机进行开发，Linux镜像使用Exbot推出的已安装好ROS的indigo 14.04版本(http://blog.exbot.net/archives/1206)
虚拟机安装时建议最少分配40G的空间。在安装完成后首先执行如下命令解决虚拟机无法上网的问题：

```
安装后不能上网的，运行以下命令：sudo ln -s /run/resolvconf/resolv.conf /etc/resolv.conf
不能挂载U盘的，试试以下命令：sudo apt-get install exfat-utils
```

更新阿里云的源并执行sudo apt-get update进行更新：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak #备份
sudo apt-get install nano
sudo nano /etc/apt/sources.list #修改

复制下列地址
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

ctrl+X保存
sudo apt-get update #更新列表
```
安装部分缺失库(可选)
```
sudo apt-get install ros-indigo-joy ros-indigo-octomap-ros 
ros-indigo-mavlink python-wstool python-catkin-tools protobuf-compiler 
libgoogle-glog-dev ros-indigo-control-toolbox ros-indigo-mavros 
```
打开终端运行roscore验证ROS系统安装正确，之后将项目(https://pan.baidu.com/s/1N7BgnEnMO_WuLCMbFMR1jQ)
目录下的文件加压拷贝到虚拟机home目录下的.gazebo的隐藏文件夹下(如无法看见则在目录下按ctrl+h即可显示隐藏文件夹)，则正确复制后的结果如下：<br>
<div align=center><img width="440" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/gazebo2.JPG"/></div>
 
复制完成后在终端中输入gazebo打开仿真器则不需要重新下载模型文件，在仿真器正确打开后应该出现如下画面而不是黑屏，同时在insert选项下有多个可选择模型：<br>
<div align=center><img width="440" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/gazebo1.JPG"/></div>
 
在验证仿真环境无误后开始安装本项目所需软件，首先重新建立一个为无人机虚拟模型专用的工作空间：
 
```
mkdir -p ~/catkin_ws_d/src
cd /catkin_ws_d/src
catkin_init_workspace
cd ..
catkin_make
source devel/setup.bash
echo $ROS_PACKAGE_PATH
```

之后将项目中/src/ardrone下的文件夹解压至空间文件目录下并用catkin_make进行编译，期间会通过网络下载和安装部分缺失库
请保证网络的畅通。在编译完成后使用source devel/setup.bash来刷新配置文件并在控制终端中输入：

```
roslaunch cvg_sim_gazebo 1.launch
```

其他测试环境如下：

命令|说明
-------------|-------------
1.launch|空白场景
3.launch|动平台无人机降落
ardrone_testworld.launch|虚拟小镇和无人机


如正确安装则可以看到仿真环境中会出现一个四旋翼飞行器并且地上布置有3个地标，如图所示：<br>
<div align=center><img width="440" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/gazebo3.JPG"/></div>
 
在新终端中输入如下命令可以进一步进行测试： <br>
``` 
rosrun image_view image_view image:=/ardrone/front/image_raw   查看前置摄像头
rosrun image_view image_view image:=/ardrone/bottom/image_raw  查看后置摄像头
rosrun image_view image_view image:=/ardrone/90/image_raw
rosrun image_view image_view image:=/ardrone/45/image_raw

rostopic pub -1 /ardrone/takeoff std_msgs/Empty 起飞
rostopic pub -1 /ardrone/land std_msgs/Empty    降落
```

使用rosrun rqt_graph rqt_graph能查看ardrone仿真模型的topic节点图：<br>
<div align=center><img width="280" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/ros1.jpg"/></div>
 

## 3.2 程序控制开发
重新建立一个为无人机控制专用的工作空间：
```
mkdir -p ~/catkin_ws_c/src
cd /catkin_ws_c/src
catkin_init_workspace
cd ..
catkin_make
source devel/setup.bash
echo $ROS_PACKAGE_PATH
```

之后将项目中/src/OLDX_simulator下的文件夹解压至空间文件目录下并用catkin_make进行编译，在编译完成后使用source devel/setup.bash来刷新配置文件并在控制终端中输入：
```
roslaunch oldx land.launch
```
则系统正常后会在新的控制台打印相关参数，此时既可以通过键盘控制飞行器，相关操控如下表所示：

键盘|说明
-------------|-------------
空格|起飞和降落
W|前进
S|后退
A|向左平飞
D|向右平飞
Shift+W|高度上升
Shift+S|高度下降
Shift+A|机头左转
Shift+D|机头右转

默认代码下为无人机动平台降落模式，此时需要运行对应的roslaunch cvg_sim_gazebo 3.launch仿真环境，正确运行后仿真环境中除了无人机外还应该存在地标
降落小车，其上加载了由我们设计的R2D复合地标(由一个缺口圆环地标和二维码组合构成)，同时会显示机载90°和45°两个相机的实时图像和地标识别结果。<br>
<div align=center><img width="300" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/ros_land1.JPG"/></div>
 
<br>
此时按键空格则飞行器起飞，起飞后动平台则开始移动其运动轨迹可在oldx_serial.cpp函数中修改car_move_sel实现直线或圆形轨迹的选择：<br>

car_move_sel|说明
-------------|-------------
0|圆形轨迹
1|直线轨迹

在起飞后无人机会自动飞行至动平台后方固定位置(around_dis=3.6 m)进行跟随移动，此时如果输入任意的前后左右移动控制量则会进入自动降落流程，飞行器会
基于图像信息逐步逼近目标并规划轨迹进行降落，在降落成功后在控制台会打印降落是否成功、降落所耗时间和降落误差等结果：<br>
<div align=center><img width="300" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/ros_land2.JPG"/></div>
<br> 
在完成降落任务后3.5s 飞行器会重新拉升到动平台后方并重新等待自动降落命令。<br>
目前的程序中已经封装了地面机器人和无人机自动控制的相关API：

car_contral|float exp_y,float exp_z,float dt,int en_auto_run,float spd,float rad
-------------|-------------
exp_y|期望前进速度  闭环
exp_z|期望旋转速度  闭环
dt|控制周期
en_auto_run|1(开环速度控制)  0(闭环速度控制)
spd|开环期望速度
rad|开环期望旋转速度

<br>

drone_contral|float exp_x,float exp_y,float exp_z,float exp_yaw,float dt,float limit
-------------|-------------
exp_x|期望x轴位置
exp_y|期望y轴位置
exp_z|期望z轴位置
exp_yaw|期望航向角
dt|控制周期
limit|速度输出限幅

<br>
程序中相关ROS节点下状态反馈和机器人控制Topic如下：

Topic|说明
-------------|-------------
velocity_cmd_drone.linear|飞行器期望机体线速度
velocity_cmd_drone.angular|飞行器期望机体角速度
velocity_cmd_car.linear|小车期望机体线速度
velocity_cmd_car.angular|小车期望机体角速度
velocity_ar_key|按键产生的期望速度
drone.pose.pose.position|飞行器当前位置
att_drone|飞行器当前姿态角
drone.twist.twist.linear|飞行器当前机体线速度
drone.twist.twist.angular|飞行器当前机角速度
car.pose.pose.position|小车当前位置
att_car|小车当前姿态角
car.twist.twist.linear|小车当前机体线速度
car.twist.twist.angular|小车当前机体角速度

<br>
另外可以快速修改宏定义#define EN_AUTO_LAND 1来实现是否使能自动降落程序，上述修改后都需要重新catkin_make来进行编译。如需自己添加
相应的控制程序和图像导航测试程序可以在oldx_serial.cpp中632行后自行添加相应代码并禁用该宏定义。

# 4 图像导航ROS开发范例(Star>50后更新)



# 5 捐赠与项目后续开发计划
如果您觉得该项目对您有帮助，也为了更好的项目推进和软硬件更新，如果愿意请通过微信捐赠该项目！
<div align=center><img width="240" height="300" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/pay.png"/></div>





