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
4.1.1 下载[数据包](https://stuhiteducn-my.sharepoint.com/:u:/g/personal/chenhao2017_stu_hit_edu_cn/EcL-m25COPpHjyleQn_TyTgB0WlWV3UL3SRkF31qfpJatQ?e=jGjfPV)  
4.1.2 在**运行建图程序前**，用一个bag记录全局地图数据
```
$ rosbag record -o out /laser_cloud_surround
```
4.1.3 之后运行LEGO_LOAM建图，建图完成后终止rosbag程序，最后会记录一个名为out_(记录时间)的.bag包。  
4.1.4 将bag转化为PCD文件  
```
$ rosrun pcl_ros bag_to_pcd  ###上一步记录的bag名称### /laser_cloud_surround  pcd
```
4.1.5 进入转化后生成的PCD文件夹，最后一个pcd文件为我们要保存的pcd文件，可以用pcl_viewer查看PCD地图文件  
4.1.6 安装ghex，并使用ghex修改PCD文件坐标轴顺序（原来坐标轴顺序为x,y,z, 修改为y,z,x）
```
$ sudo apt-get install ghex
```
### 4.2 运行定位程序
使用gedit打开launch文件并修改  
4.2.1 修改地图的名称   
```
<param name="globalmap_pcd" value="$(find hdl_localization)/data/###修改为自己的地图PCD名称###" />
```
4.2.2 修改IMU参数
```
<param name="use_imu" value="false" />
```
4.2.3 运行程序
```
$ rosparam set use_sim_time true
$ roslaunch hdl_localization hdl_localization.launch
$ roscd hdl_localization/rviz
$ rviz -d hdl_localization.rviz
$ rosbag play --clock ###下载的数据包名称###
```