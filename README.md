# upgrade_limo
본 gihub 레파지토리는 Limo ROS1 버전을 사용하고 계신 고객들 께서 ROS2로 업그레이드를 하는 벙법에 대해서 초기 세팅을 도와드리는데에 초점이 잡혀진 내용입니다.

## 지원 내용
본 gihub 레파지토리에서는 다음과 같은 내용을 다루고 있습니다.
* Jetson Nano에 Ubuntu 20.04 설치 링크 안내
* ROS2 Foxy 설치 링크 안내
* Limo 사용을 위한 Ubuntu 시스템 세팅 방법 안내
* Ydlidar SDK 설치 안내
* Limo, lidar 관련 Package 제공
* Limo, Imu, Lidar, Camera 데이터 확인 방법

### Jetson Nano Ubuntu 20.04 설치
Jetson Nano는 Ubuntu 18.04까지만 지원을 하지만, 비공식 적으로 Ubuntu 20.04를 설치 할 수 있도록 이미지가 있습니다. 다음 링크를 참고하여 SD 카드에 Jetson Nano용 SD 카드에 설치해 주시기 바라겠습니다.

    https://github.com/Qengineering/Jetson-Nano-Ubuntu-20-image

### ROS2 Foxy 설치
Ubuntu 20.04를 설치해 주신 후 ROS2 Foxy 버전을 Ubuntu(Debian)으로 설치해 주시면 되시겠습니다. 관련하여 자세한 설치 방법은 다음 링크를 참고해 주시면 감사하겠습니다.

    https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html

### Ubuntu 시스템 세팅
Ubuntu 시스템 세팅에는 Limo의 통신 포트와 Ydlidar 통신 포트에 대해서 고정을 하는 방법에, Vscode 및 Nomachine과 같이 쉽게 설치를 진행 할 수 있는 소프트웨어를 설치하는 방법에 대해서 알려드립니다.


1. vscode install
```
    $ cd ~/
    $ git clone https://github.com/WeGo-Robotics/upgrade_limo.git
    $ cd ~/upgrade_limo/system_setting/installVSCode
    $ ./installVSCode.sh
```
2. nomachine install
```
    $ cd ~/upgrade_limo/system_setting
    $ sudo dpkg -i nomachine_8.11.3_3_arm64.deb
```
3. Port Setting
```
    $ cd ~/upgrade_limo/system_setting/udev_setting
    $ sudo chmod 777 ./*
    $ ./set_port.sh
    $ systemctl stop nvgetty
    $ systemctl disable nvgetty
    $ reboot
```
reboot를 진행 한 후,  ls /dev/ttylimo 와, ls /dev/ydlidar 라는 포트가 뜨게 되면 설정이 잘 된 것입니다.

### YDLidar SDK 세팅
Ydlidar SDK를 설치합니다.
```
    $ cd ~/upgrade_limo/YDLidar-SDK
    $ mkdir build
    $ cd build
    $ cmake ..
    $ make -j4
    $ sudo make install
```

### Limo, Lidar ROS2 Package 세팅
홈디렉토리에 Limo와 Lidar ROS2 Package를 세팅하고 빌드합니다.

```
    $ mkdir -p ~/ros2_ws/src
    $ cd ~/upgrade_limo/
    $ mv limo_ros2 ~/ros2_ws/src/
    $ mv ydlidar_ros2_driver ~/ros2_ws/src/
    $ cd ~/ros2_ws
    $ colcon build
    $ source install/setup.bash
    $ echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

### Limo ROS2 활용
Limo, Lidar, camera 를 ros2로 제어하고 sensor data를 확인하는 방벙에 대해서 설명합니다.
1. limo ROS2
```
    $ ros2 launch limo_base limo_base.launch.py
    $ ros2 topic list -t
```
![Screenshot from 2024-05-08 17-59-25](https://github.com/WeGo-Robotics/upgrade_limo/assets/150217205/1cbfb950-9467-4a66-961c-905221f75f46)

각각의 topic은 다음을 의미합니다.
* /cmd_vel: Limo를 제어하는 topic 입니다.
* /imu: Limo의 Imu 센서 토픽입니다.
* /limo_status: Limo의 배터리 상태, 모드, 모터 RPM 등을 나타내는 topic입니다.

Limo는 다음과 같이 제어 할 수 있습니다. (직진속도 0.3 회전 속도 0.3)으로 Limo 제어
```
    $ ros2 topic pub -r 10 /cmd_vel geometry_msgs/msg/Twist "linear:
        x: 0.3
        y: 0.0
        z: 0.0
    angular:
        x: 0.0
        y: 0.0
        z: 0.3
```

2. Lidar ROS2
```
    $ ros2 launch ydlidar_ros2_driver ydlidar_launch.py 
    $ ros2 topic list -t
```

![Screenshot from 2024-05-08 18-33-19](https://github.com/WeGo-Robotics/upgrade_limo/assets/150217205/5fde5347-f17a-41de-9e2a-67b1e502f8a3)

각각의  토픽은 다음의 의미합니다.
* /scan: Lidar 센서 데이터

Lidar 토픽은 rviz2를 활용하여 확인 할 수 있습니다.
![Screenshot from 2024-05-08 18-32-30](https://github.com/WeGo-Robotics/upgrade_limo/assets/150217205/75946c6e-d514-4816-9204-dddc9c6e216f)

3. Camera ROS2
카메라 데이터의 경우에는 Image Data만 나오게 구성되어 있으며 다음과 같이 실행 할 수 있습니다.
```
    $ ros2 run image_tools cam2image 
    $ ros2 topic list -t
```
![Screenshot from 2024-05-08 18-36-19](https://github.com/WeGo-Robotics/upgrade_limo/assets/150217205/25a2a037-45bc-486c-beb0-7d4b06d98e63)

각각의  토픽은 다음의 의미합니다.
* /image: camera 센서 데이터

Camera data는 rqt_image_view를 통해서 확인 가능합니다.
![Screenshot from 2024-05-08 18-37-38](https://github.com/WeGo-Robotics/upgrade_limo/assets/150217205/c7d6f124-2b3d-4921-bc2b-bbc8974f4733)
