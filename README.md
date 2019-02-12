
# 1 OLDX-开源多旋翼开发平台项目  
<div align=center><img width="600" height="130" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/logo.JPG"/></div>
<div align=center><img width="440" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/fc.jpg"/></div>
  OLDX多旋翼开发平台（OLDX-FC）是由北京理工大学自动化学院所属《北理云逸科技》团队开发的一个目前国内最完整的免费开源飞控项目，随着国内开源飞控的逐步发展如匿名、
INF、无名和ACFly飞控的陆续推出，如光流、气压计和GPS等相关算法已经逐步完善，但是相比Pixhawk等国外开源飞控平台的发展和定位仍然有发展空间。OLDX-FC于14年开始对多旋翼飞行器进行研究期间也经历过开源和借鉴的过程，为希望进一步推动国内开源飞控协作开发和
相互学习、相互分享的趋势，团队将该OLDX-FC转化为开源项目，采用自由捐赠的形式继续发展
[捐赠地址](https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/pay.png)
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
打开终端运行roscore验证ROS系统安装正确，之后将项目(https://pan.baidu.com/s/1GuXCdIKfUrpBHTj5R96flw)
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
<br>
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
<div align=center><img width="300" height="280" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file_sim/ros1.jpg"/></div>
 

# 4 测试与开发说明
roslaunch cvg_sim_gazebo 3.launch
roslaunch oldx land.launch

# 8 捐赠与项目后续开发计划
如果您觉得该项目对您有帮助，也为了更好的项目推进和软硬件更新，如果愿意请通过微信捐赠该项目！
<div align=center><img width="240" height="300" src="https://github.com/golaced/OLDX_DRONE_SIM/blob/rmd/support_file/img_file/pay.png"/></div>





