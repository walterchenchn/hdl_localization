# hdl_localization安装指南（定位）
## 1. 依赖
### 1.1 PCL 1.7
```
$ sudo apt-get install libpcl-dev
```
### 1.2 [ndt_omp](https://github.com/koide3/ndt_omp)
```
$ cd ~
$ mkdir -p catkin_ws/src
$ cd catkin_ws/src
$ git clone https://github.com/koide3/ndt_omp.git
```

## 2. 下载及编译
```
$ cd catkin_ws/src
$ git clone https://github.com/koide3/hdl_localization.git
$ cd ..
$ catkin_make
```

## 3. 运行
```
$ rosparam set use_sim_time true
$ roslaunch hdl_localization hdl_localization.launch
$ roscd hdl_localization/rviz
$ rviz -d hdl_localization.rviz
```
下载数据包[hdl_400.bag.tar.gz](http://www.aisl.cs.tut.ac.jp/databases/hdl_graph_slam/hdl_400.bag.tar.gz)，并播放
```
$ rosbag play --clock hdl_400.bag
```
#### 注意：程序会占用大量CPU计算资源，请尽量在台式机等CPU性能较强的平台上运行本程序。

## 4. 使用自定义数据包
### 4.1 使用[LEGO_LOAM](https://github.com/walterchenchn/LeGO-LOAM)建图并保存
下载[数据包]()  
在**运行建图程序前**，用一个bag记录全局地图数据，之后将bag转化为PCD文件
```
$
```
得到使用READ修改PCD文件坐标轴顺序（y,z,x）
```
$
```
### 4.2 运行定位程序
修改launch文件，将地图的地址改为自定义的地址
```
$ gedit 
```
运行程序
```
$ rosparam set use_sim_time true
$ roslaunch hdl_localization hdl_localization.launch
$ roscd hdl_localization/rviz
$ rviz -d hdl_localization.rviz
```