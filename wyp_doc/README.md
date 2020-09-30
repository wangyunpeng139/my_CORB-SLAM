



# 1 run





```shell
catkin_make -DCATKIN_WHITELIST_PACKAGES="corbslam_client"
catkin_make -DCATKIN_WHITELIST_PACKAGES="corbslam_server"
```



**修改CMaKeList.txt文件**



```shell

	# skylor : 2020-09-23
	/usr/include/pcl-1.8
	/home/wyp/corb_ws/devel/include/corbslam_client
	/home/wyp/corb_ws/devel/include/corbslam_server
	
find_package(OpenCV 3.4 REQUIRED)
set(Eigen3_DIR  /usr/local/share/eigen3/cmake )
find_package(Eigen3 3.3.1 REQUIRED)
set(Pangolin_DIR  /home/wyp/mylib/Pangolin/lib/cmake/Pangolin )
find_package(Pangolin REQUIRED)
```



**docker相关的容器命令**

```shell
docker run -itv /home/wyp/corb_ws/:/root/corb_ws/ 13939/ros:indigo_vnc_ssh bash

echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
echo "source /root/corb_ws/devel/setup.bash">>~/.bashrc
source ~/.bashrc
```





# 2 change Log







* 2020-09-29

  > - [ ] 打开一个容器，符合`corb-slam`的环境配置
  >
  >   > ```shell
  >   > docker run -itv /home/wyp/corb_ws/:/root/corb_ws/ 13939/ros:indigo_vnc_ssh bash
  >   > ```
  >   >
  >   > 



