**原文地址**

https://github.com/lifunudt/CORB-SLAM







# 2 Planning



- [x] Complete the compilation  of `corb-slam` on the `ubuntu18.04` (date: 2020-09-30)
- [ ] The whole project uses a version of `Eigen3` (version = 3.3.1)
- [ ] Take out all the `msg` and compile them separately









# 3 run



```shell

```







# 4 change Log

* 2020-09-30

  > - [x] Complete the compilation of `corb-slam` on the `ubuntu18.04`
  > - [ ] 









# 10 环境配置

eigen3 : version=3.3.1 and 3.2.0

opencv: version=3.4.6

pcl: version=1.8



## 10.1 Install Pangolin(for ubuntu14,04)

```shell
#使用docker镜像，ubuntu14.04 ，Pangolin要使用旧版本的

$ sudo apt-get install libglew-dev   #安装Glew
$ sudo apt-get install cmake         #安装CMake
$ sudo apt-get install libboost-dev libboost-thread-dev libboost-filesystem-dev  #安装Boost
 
#下载、编译、安装Pangolin:
$ git clone https://github.com/zzx2GH/Pangolin.git
$ cd Pangolin
$ mkdir build
$ cd build
$ cmake -DCPP11_NO_BOOST=1 ..
$ make
$ sudo make install
```

## 10.2 about eigen3

```shell
#eigen3
# corb-slam使用 3.2.0，第三方库g2o里面使用3.3.1
cmake -DCMAKE_INSTALL_PREFIX=/root/corb_ws/src/eigen_install ..
git checkout 3.2.0
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/wyp/corb_ws/src/eigen_install ..
make && make install
#本机正常安装
/usr/local/include/eigen3
git checkout 3.3.1
mkdir build && cd build
cmake ..
make && make install
```

## 10.3 modified



**修改CMaKeList.txt文件**



**Client**

```shell
set(OpenCV_DIR /home/wyp/mylib/opencv346/share/OpenCV)
find_package(OpenCV 3.4 REQUIRED)
# set(Eigen3_DIR  /usr/local/share/eigen3/cmake )
# find_package(Eigen3 3.3.1 REQUIRED)
set(Pangolin_DIR  /home/wyp/mylib/Pangolin/lib/cmake/Pangolin )
find_package(Pangolin REQUIRED)

include_directories(
        ${catkin_INCLUDE_DIRS}
        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/include
        # ${EIGEN3_INCLUDE_DIR}
        ${PROJECT_SOURCE_DIR}/../eigen_install/include/eigen3

        ${Pangolin_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIR}
        ${PROJECT_SOURCE_DIR}/Examples/RGB-D
	# skylor : 2020-09-23
	/usr/include/pcl-1.8
	${PROJECT_SOURCE_DIR}/../../../devel/include/corbslam_client
        ${PROJECT_SOURCE_DIR}/../../../devel/include/corbslam_server
)
```

**Server**

```shell
set(OpenCV_DIR /home/wyp/mylib/opencv346/share/OpenCV)
find_package(OpenCV 3.4 REQUIRED)
# set(Eigen3_DIR  /usr/local/share/eigen3/cmake )
# find_package(Eigen3 3.3.1 REQUIRED)
set(Pangolin_DIR  /home/wyp/mylib/Pangolin/lib/cmake/Pangolin )
find_package(Pangolin REQUIRED)

include_directories(
        ${catkin_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIR}
        # ${EIGEN3_INCLUDE_DIR}
        ${PROJECT_SOURCE_DIR}/../eigen_install/include/eigen3

        ${Pangolin_INCLUDE_DIRS}

        ${PROJECT_SOURCE_DIR}/../corbslam_client
        ${PROJECT_SOURCE_DIR}/../corbslam_client/include

        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/include
	# skylor : 2020-09-23
	/usr/include/pcl-1.8
	${PROJECT_SOURCE_DIR}/../../../devel/include/corbslam_client
        ${PROJECT_SOURCE_DIR}/../../../devel/include/corbslam_server

)
```



## 10.4 start to compile





```shell
# 第一条命令会报错，不用例会，继续执行后两条
catkin_make -DCATKIN_WHITELIST_PACKAGES="corbslam_client"
catkin_make -DCATKIN_WHITELIST_PACKAGES="corbslam_server"
catkin_make -DCATKIN_WHITELIST_PACKAGES="corbslam_client"
```







# 原作者README如下



# --------

# CORB-SLAM

**Authors:** Fu Li(lifu11@nudt.edu.cn),Shaowu Yang(shaowu.yang@nudt.edu.cn)

**Current version:** 1.0.0

## introduction

**CORB-SLAM(collaborative ORB-SLAM)** is a centralized multi-robot visual SLAM system based on  **[ORB_SLAM2[1-2]](https://github.com/raulmur/ORB_SLAM2)**.
The ORB-SLAM2 is a versatile and accurate visual SLAM method that has been popularly applied in single robot applications. However, this method cannot provide support to multi-robot cooperation in environmental mapping.
The CORB-SLAM system consists of multiple ORB_SLAM2 clients for local mapping and a central server for global map fusion. Specifically, we extend each of the ORB_SLAM2 clients with a memory managing module that organizes the local map and communicates with the central server. In the central server, we detect the overlaps of multiple local maps by the DBoW method, and fuse these maps by utilizing the PnP method and global optimization through bundle adjustment.

<!-- <div align=center> <img src="https://github.com/lifunudt/M2SLAM/blob/master/images/framework.png" alt="M2SLAM" height="180" align=center /> </div> -->

## 0. Related Publications

[1] F. Li, S. Yang, X. Yi, X. Yang. CORB-SLAM: a Collaborative Visual SLAM System for Multiple Robots. submitted.

## 1. Prerequisites

### 1.0 requirements
  * ubuntu 14.04
  * ROS indigo
  * ORBSLAM2 1.0.0
  * boost

### 1.1 ROS install

Inatall ROS indigo according to the instructions in [ROS wiki](http://wiki.ros.org/indigo/Installation).

### 1.2 ORBSLAM2 and its dependencies

Our CORB-SLAM system is build on the foundation of [ORB_SLAM2(https://github.com/raulmur/ORB_SLAM2). You should follow the instructions provided by ORB_SLAM2 build its dependencies. We do not list here.

### 1.3 Boost library install
We use boost library to serialize and deserialize the data.
We can install boost library using the following instruction in terminal.
```bash
sudo apt-get instal libboost-dev
```


### 2.1 build CORB-SLAM

The CORB-SLAM runs as the ROS package. and the CORB-SLAM *src* directory should be the ROS package directory, *catkin_src/*.
We use catkin tool to organize the CORB-SLAM packages, corbslam_client and corbslam_servcer.

Terminal in the *catkin_src/* directory.
```
catkin_make
catkin_install
```

### 2.2 run CORB-SLAM

#### 2.2.1 start ros core
```
roscore
```
#### 2.2.2 start corbslam_servcer
```
rosrun corbslam_servcer PATH_TO_VOCABULARY/ORBvoc.txt PATH_TO_CONFIGURATION/KITTIX.yaml
```
#### 2.2.3 run multiple corbslam_client in different datasets

1. run KITTI datasets

Download the dataset (grayscale images) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php

Execute the following command. Change `KITTIX.yaml`to KITTI00-02.yaml, KITTI03.yaml or KITTI04-12.yaml for sequence 0 to 2, 3, and 4 to 12 respectively. Change `PATH_TO_DATASET_FOLDER` to the uncompressed dataset folder. Change `SEQUENCE_NUMBER` to 00, 01, 02,.., 11.

```
rosrun corbslam_client_stereo_kitti corbslam_client_stereo_kitti Vocabulary/ORBvoc.txt Examples/Stereo/KITTIX.yaml PATH_TO_DATASET_FOLDER CLIENT_ID
```

## Reference
[1] Mur-Artal R, Montiel J M M, Tardos J D. ORB-SLAM: a versatile and accurate monocular SLAM system[J]. IEEE Transactions on Robotics, 2015, 31(5): 1147-1163.

[2] Mur-Artal R, Tardos J D. ORB-SLAM2: an Open-Source SLAM System for Monocular, Stereo and RGB-D Cameras[J]. arXiv preprint arXiv:1610.06475, 2016.

## License
CORB-SLAM is released under a [GPLv3 license](https://github.com/lifunudt/M2SLAM/blob/master/License-gpl.txt).

For a closed-source version of M2SLAM for commercial purposes, please contact the authors.
