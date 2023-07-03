# scale_truck_control_ros2
scale_truck_control ros2 version

# Develop History
**2023.05.24**
```
Implemented dual ROI version for normal and lane-changing modes

- Added distinction between normal mode and lane change mode using two versions of ROI.
- Established lane change process: CMD -> FV2 lane change command -> FV2 lane change complete -> CMD -> FV1 lane change command -> FV1 lane change complete -> CMD -> LV lane change command -> LV lane change complete -> CMD.
- Implemented controller button toggle upon completion of lane change.
```

# Set OpenCV 4.4.0
- Uninstall old version of OpenCV
```
sudo apt-get purge  libopencv* python-opencv
sudo apt-get autoremove
sudo find /usr/local/ -name "*opencv*" -exec rm -i {} \;
```
- Install 4.4.0 version of OpenCV
```
sudo apt-get update
sudo apt-get upgrade

sudo apt-get -y install build-essential cmake
sudo apt-get -y install pkg-config
sudo apt-get -y install libjpeg-dev libtiff5-dev libpng-dev
sudo apt-get -y install ffmpeg libavcodec-dev libavformat-dev libswscale-dev libxvidcore-dev libx264-dev libxine2-dev
sudo apt-get -y install libv4l-dev v4l-utils
sudo apt-get -y install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev 
sudo apt-get -y install libgtk-3-dev
sudo apt-get -y install mesa-utils libgl1-mesa-dri libgtkgl2.0-dev libgtkglext1-dev
sudo apt-get -y install libatlas-base-dev gfortran libeigen3-dev
sudo apt-get -y install python3-dev python3-numpy
```
- Download OpenCV 4.4.0 source file
```
mkdir OpenCV && cd OpenCV
git clone -b 4.4.0 https://github.com/opencv/opencv
git clone -b 4.4.0 https://github.com/opencv/opencv_contrib
cd opencv && mkdir build && cd build
```
- Build OpenCV 4.4.0
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
-D WITH_OPENCL=OFF \
-D WITH_CUDA=ON \
-D CUDA_ARCH_BIN=7.2 \
-D CUDA_ARCH_PTX="" \
-D WITH_CUDNN=ON \
-D WITH_CUBLAS=ON \
-D ENABLE_FAST_MATH=ON \
-D CUDA_FAST_MATH=ON \
-D OPENCV_DNN_CUDA=ON \
-D ENABLE_NEON=ON \
-D WITH_QT=OFF \
-D WITH_OPENMP=ON \
-D WITH_OPENGL=ON \
-D BUILD_TIFF=ON \
-D WITH_FFMPEG=ON \
-D WITH_GSTREAMER=ON \
-D WITH_TBB=ON \
-D BUILD_TBB=ON \
-D BUILD_TESTS=OFF \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D BUILD_NEW_PYTHON_SUPPORT=ON \
-D BUILD_opencv_python3=TRUE \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D BUILD_EXAMPLES=OFF \
..
```
```
sudo make install -j8
```

# Jetson Stats
```
sudo -H pip3 install jetson-stats
jetson_release
```

# Set cv_bridge
- Install vision_opencv
```
cd ~/ros2_ws/src
git clone -b galactic https://github.com/ros-perception/vision_opencv.git
```
- cv_bridge Setup
```
cd /vision_opencv/cv_bridge
vim CMakelist.txt
```
```
--find_package(OpenCV 4 QUIET
++find_package(OpenCV 4.4 QUIET
```
```
cd ~/ros2_ws
colcon build --packages-select vision_opencv
```

# Install Micro-ros-Aruduino
- 본인의 ROS2 워크스페이스에서 작업하면 됩니다.
```
source /opt/ros/galactic/setup.bash
cd ~/ros2_ws/src 
git clone https://github.com/micro-ROS/micro_ros_setup.git -b galactic
```

- rosdep으로 의존 패키지 업데이트
```
sudo apt update && rosdep update
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -y
```

- pip 설치
```
sudo apt-get install python3-pip
```

- micro-ROS tools 과 소스 빌드
```
cd ~/ros2_ws
colcon build --packages-select micro_ros_setup
source install/local_setup.bash
```

- micro-ROS agent 패키지 설치
```
cd ~/ros2_ws
ros2 run micro_ros_setup create_agent_ws.sh
ros2 run micro_ros_setup build_agent.sh
source install/local_setup.bash
```

- Test
```
sudo chmod 777 /dev/ttyACM0
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyACM0
```

# ROS2 Packages Install
- ros2_msg
```
cd ~/ros2_ws/src
git clone https://github.com/AveesLab/ros2_msg.git
cd ~/ros2_ws && colcon build --symlink-install && . install/setup.bash
```

- usb_cam
```
sudo apt-get install ros-galactic-usb-cam
cd ~/ros2_ws/src
git clone https://github.com/ros-drivers/usb_cam.git -b ros2
```

- rplidar_ros2
```
cd ~/ros2_ws/src
git clone https://github.com/CarlDegio/rplidar_ros.git -b ros2
```

- laser_filter
```
sudo apt-get install ros-galactic-filters
sudo apt-get install ros-galactic-angles
sudo apt-get install ros-galactic-laser-geometry
sudo ln -s /usr/include/eigen3/Eigen  /usr/include/Eigen
cd ~/ros2_ws/src
git clone https://github.com/wonseokkkk/laser_filters.git
```

- object_detection
```
sudo apt-get install ros-galactic-perception-pcl
cd ~/ros2_ws/src
git clone https://github.com/AveesLab/object_detection_ros2.git
```

- lane_detection
```
cd ~/ros2_ws/src
git clone https://github.com/AveesLab/lane_detection_ros2.git
```
- scale_truck_control_ros2
```
cd ~/ros2_ws/src
git clone https://github.com/AveesLab/scale_truck_control_ros2.git
```


# alias
```
sudo vim ~/.bashrc
```
```
alias cw='cd ~/ros2_ws/src'
alias cb='source ~/ros2_ws/install/setup.bash'
alias sb='source ~/.bashrc'
alias cm='cd ~/ros2_ws && colcon build --symlink-install && . install/setup.bash'
#alias cm='cd ~/ros2_ws && colcon build --packages-select scale_truck_control_ros2  --symlink-install && . install/setup.bash'
alias eb='sudo vim ~/.bashrc'
```
