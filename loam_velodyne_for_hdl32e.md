---
title: 修改loam_velodyne使其能够兼容HDL-32E
date: 2017-10-09 10:35:53
tags:
---

# 0.前言
LOAM是一个激光雷达SLAM算法，由[Ji Zhang](http://www.frc.ri.cmu.edu/~jizhang03/)博士等提出并实现。然而Github上的公开Repo，[loam_velodyne](https://github.com/laboshinl/loam_velodyne)，原工程仅支持Velodyne VLP-16型多线激光雷达，无法直接使用32线等运行算法。这里我们参考原Repo中的[Issue #10](https://github.com/laboshinl/loam_velodyne/issues/10)对工程进行修改，使其能够支持HDL-32E。

# 1.安装loam_velodyne
直接按照[loam_velodyne](https://github.com/laboshinl/loam_velodyne)主页的指示来即可。
```
$ cd ~/catkin_ws/src/
$ git clone https://github.com/laboshinl/loam_velodyne.git
$ cd ~/catkin_ws
$ catkin_make -DCMAKE_BUILD_TYPE=Release 
```
# 2.修改工程
## 修改`scanRegistration.cpp`

首先修改代码，使用点云类型`velodyne_pointcloud::PointXYZIR`，将每一线激光构造成一个scan，拥有一个`scanID`:
```
  std::vector<int> scanStartInd(N_SCANS, 0);
  std::vector<int> scanEndInd(N_SCANS, 0);
  double timeScanCur = laserCloudMsg->header.stamp.toSec();
  pcl::PointCloud<pcl::PointXYZI> laserCloudIn;
  pcl::PointCloud<velodyne_pointcloud::PointXYZIR> laserCloudIn_vel;

  pcl::fromROSMsg(*laserCloudMsg, laserCloudIn);
  pcl::fromROSMsg(*laserCloudMsg, laserCloudIn_vel);

  std::vector<int> indices;
  pcl::removeNaNFromPointCloud(laserCloudIn, laserCloudIn, indices);

  int cloudSize = laserCloudIn.points.size();
  float startOri = -atan2(laserCloudIn.points[0].y, laserCloudIn.points[0].x);
  float endOri = -atan2(laserCloudIn.points[cloudSize - 1].y,
                        laserCloudIn.points[cloudSize - 1].x) + 2 * M_PI;

  if (endOri - startOri > 3 * M_PI) {
    endOri -= 2 * M_PI;
  } else if (endOri - startOri < M_PI) {
    endOri += 2 * M_PI;
  }
  bool halfPassed = false;
  int count = cloudSize;
  PointType point;
  std::vector<pcl::PointCloud<PointType> > laserCloudScans(N_SCANS);
  for (int i = 0; i < cloudSize; i++) {
    point.x = laserCloudIn.points[i].y;
    point.y = laserCloudIn.points[i].z;
    point.z = laserCloudIn.points[i].x;

    int scanID;
   /* float angle = atan2(point.y,sqrt(point.x * point.x + point.z * point.z)) * 180 / M_PI;
    int roundedAngle = int(angle + (angle<0.0?-0.5:+0.5)); 
    std::cout << angle << std::endl;
    if (roundedAngle > 0){
      scanID = roundedAngle;
    }
    else {
      scanID = roundedAngle + (N_SCANS - 1);
    }*/
    scanID = laserCloudIn_vel.points[indices[i]].ring;
    if (scanID > (N_SCANS - 1) || scanID < 0 ){
      count--;
      continue;
    }
```
修改数组大小，[HDL-32E](http://www.velodynelidar.com/hdl-32e.html)一次扫描会产生70000个点，故将数组大小定为100000:
```
float cloudCurvature[100000];
int cloudSortInd[100000];
int cloudNeighborPicked[100000];
int cloudLabel[100000];
```
修改`N_SCANS`为32:
```
N_SCANS = 32;
```
增加`velodyne_pointcloud::PointXYZIR`所需的头文件:
```
#include <velodyne_pointcloud/point_types.h> 
#include <velodyne_pointcloud/rawdata.h>
```

## 修改`CMakeLists.txt`和`package.xml`

在`CMakeLists.txt`中:
```
find_package(
...
velodyne_pointcloud

)

```
在`package.xml`中增加:
```
<build_depend>velodyne_pointcloud</build_depend>
...
...
<run_depend>velodyne_pointcloud</run_depend>
```

# 3.安装Velodyne驱动
安装Velodyne的ROS驱动
```
$ sudo apt-get install ros-indigo-velodyne
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ros-drivers/velodyne.git
$ cd ~/catkin_ws
$ catkin_make
```

# Reference

[https://github.com/laboshinl/loam_velodyne/issues/10](https://github.com/laboshinl/loam_velodyne/issues/10)



