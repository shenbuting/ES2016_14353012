# Ubuntu中ROS的安装以及cartographer的安装

## （一）ROS安装
> 添加 sources.list, 配置自己的电脑使其能够安装来自 packages.ros.org的软件包：  
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'   
> 添加 keys：  
$ sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
![1.png](https://ooo.0o0.ooo/2016/11/11/58259bdb73855.png) 
![2.png](https://ooo.0o0.ooo/2016/11/11/58259ce21525d.png)  
> 安装，确保你的Debian软件包索引是最新的：  
$ sudo apt-get update  
![3.png](https://ooo.0o0.ooo/2016/11/11/58259d6fd3e3e.png)    
> 桌面完整版安装：  
$ sudo apt-get install ros-jade-desktop-full  
> 初始化 rosdep:  
$ sudo rosdep init  
$ rosdep update  
![4.png](https://ooo.0o0.ooo/2016/11/11/5825b69271bcf.png)  
> 这里有一个报错，解决如下：  
![5.png](https://ooo.0o0.ooo/2016/11/11/5825b6c54e4ae.png)  
![6.png](https://ooo.0o0.ooo/2016/11/11/5825b718d98e5.png)  

> 环境配置  
> 每次打开一个新的终端时ROS环境变量都能够自动配置好（即添加到bash会话中）：  
$ echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc
![7.png](https://ooo.0o0.ooo/2016/11/11/5825b7c24fb71.png)  
![8.png](https://ooo.0o0.ooo/2016/11/11/5825b7c8c7ce2.png)  
> 最后获取安装包源码：  
$ apt-get source ros-jade-laser-pipeline
> 这一步似乎没有成功。
![9.png](https://ooo.0o0.ooo/2016/11/11/5825b8bca257a.png)

> 我们来测试一下：  
$ roscore  
![24.png](https://ooo.0o0.ooo/2016/11/11/5825c3183e36e.png)  
> 这里也会有报错，似乎是无法访问自己的主机，解决如下：  
> 查看自己的ip，加入进bashrc文件的末尾:  
![28.png](https://ooo.0o0.ooo/2016/11/11/5825c3186e0fb.png)  
> 所以我在bashrc中加入了：  
$ export ROS_IP=192.168.143.128  

> 再打开一个Terminal,输入下面的命令.开启一个小乌龟界面：  
$ rosrun turtlesim turtlesim_node  
![25.png](https://ooo.0o0.ooo/2016/11/11/5825c3182bfeb.png)  
> 再打开一个Terminal,输入下诉命令.接受键盘输入,控制小乌龟移动   
$ rosrun turtlesim turtle_teleop_key  
> 选中最后打开的Terminal,键盘按下上下左右按键,可看到控制小乌龟移动   
![26.png](https://ooo.0o0.ooo/2016/11/11/5825c3183fb53.png)  
> 再打开一个Terminal,输入下诉命令,可以看到当前ROS nodes以及Topic等图形展示  
$ rosrun rqt_graph rqt_graph  
![27.png](https://ooo.0o0.ooo/2016/11/11/5825c31872c6d.png)  
> 左右两边矩形为ROS node,中间连线上是Topic名称。   

## （二）cartographer的安装
> 安装wstool和rosdep：  
$ sudo apt-get update	（截图同上）  
$ sudo apt-get install -y python-wstool python-rosdep ninja-build  
![10.png](https://ooo.0o0.ooo/2016/11/11/5825bc6e22501.png)  
$ mkdir catkin_ws  
$ cd catkin_ws  
$ wstool init src  
> 这几步好像还是看的原始文档，截图稍微有点乱：   
![11.png](https://ooo.0o0.ooo/2016/11/11/5825bd6da67e7.png)  
$ wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall  
![12.png](https://ooo.0o0.ooo/2016/11/11/5825bdcbd0b2c.png)  
$ wstool update -t src  
![13.png](https://ooo.0o0.ooo/2016/11/11/5825bdcd3bfec.png)  
$ rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y  
![14.png](https://ooo.0o0.ooo/2016/11/11/5825be5f216cc.png)    
> 此处失败多次，后来才看到公告，开始补坑：
$ git clone https://github.com/hitcm/ceres-solver-1.11.0.git  
$ cd ceres-solver-1.11.0/build    
$ cmake ..    
$ make  
![15.png](https://ooo.0o0.ooo/2016/11/11/5825bf7e378cb.png)  
$ sudo make install  
![16.png](https://ooo.0o0.ooo/2016/11/11/5825bfa031a8c.png)  
$ ninja  
![17.png](https://ooo.0o0.ooo/2016/11/11/5825bfb75f987.png)   
$ ninja test   
![18.png](https://ooo.0o0.ooo/2016/11/11/5825bff5b4069.png)  
$ sudo ninja install  
![19.png](https://ooo.0o0.ooo/2016/11/11/5825c02b59dcc.png)  
$ git clone https://github.com/hitcm/cartographer_ros.git   
> 然后到catkin_ws下面运行catkin_make即可:
![20.png](https://ooo.0o0.ooo/2016/11/11/5825c123cd123.png)   
> 数据测试：  
$ wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag   
> 然后运行launch文件:    
$ roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
> 其中会出现这个报错：
![21.png](https://ooo.0o0.ooo/2016/11/11/5825c1a0c6af9.png)   
> 解决如下：    
> 虽然其实我直接运行这个也是不行的，blog上提供的两种方式都搞了几次也不有反应，我的解决方法是：  
> 读取当前catkin工作空间的环境变量：source devel/setup.sh      
![22.png](https://ooo.0o0.ooo/2016/11/11/5825c1e16f061.png)

> 最终效果图（2d），3d实在下不动了不想跑了：
![23.png](https://ooo.0o0.ooo/2016/11/11/5825c28351bfb.png)  



