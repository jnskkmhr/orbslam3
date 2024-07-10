# ORB_SLAM3_ROS2
This repository is the modified package of the [original repository](https://github.com/zang09/ORB_SLAM3_ROS2). \
The issue I had is 
* rclcpp node does not provide anything (tf2, tracked image, pose topics etc)
* stereo-inertial node did not work because of QOS setting in IMU subscriber.
* SLAM pose was optical frame pose expressed in OpenCV frame, but I needed camera frame pose in ROS FLU coordinate. 
---

## Prerequisites
Current repository supports:
  - Ubuntu 20.04
  - ROS2 foxy
  - OpenCV 4.2.0

In the future, I will also test with Ubuntu22.04 (OpenCV 4.5.4).

## How to build
We use docker for the simplicity. \
You can build docker image via
```bash
./docker/build_image.sh
```

Then, run container
```bash
./docker/run_container.sh
```

To build orbslam3 ros2, 
```bash
colcon build --cmake-args -DCMAKE_CXX_FLAGS="-w" --symlink-install --packages-select orbslam3
```

## How to use
1. Source the workspace  
```
$ source /home/ros2_ws/install/setup.bash
```

2. Run orbslam mode, which you want.  

Currently, I modified stereo and stereo-inertial node. \
First, start realsense node. \
For example, to run stereo/stereo-inertial, 
```bash
ros2 launch realsense2_camera rs_launch.py enable_infra1:=true enable_infra2:=true enable_accel:=true enable_gyro:=true unite_imu_method:=2 infra_width:=640 infra_height:=480 camera_name:=d455 camera_namespace:=d455
```

Stereo node:
```bash
ros2 launch orbslam3 stereo_d455.launch.yaml
```

Stereo-Inertial node:
```bash
ros2 launch orbslam3 stereo_inertial_d455.launch.yaml
```

### WIP
Currently, stereo-inertial node does not work properly. \
I see IMU initialization takes a lot of time, and the pose sometimes jumps. \
This might be because of wrong calibration configuration. \
Please send PR if you know how to solve issues.