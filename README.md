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
```


### YDLidar SDK 세팅