## Upgrade Limo ROS1 Model to ROS2 with Ubuntu 20.04

> [!WARNING]
> For further ROS2 upgrade, it is recommended to use Docker containers instead of upgrading Jetson Nano, due to its video driver incompatibility on the latest Ubuntu distributions.
> Please check out https://github.com/dusty-nv/jetson-containers or simillar projects.

#### Prerequisites
1. Ensure you have an Agilex Limo ROS1 Model with Nvidia Jetson Nano.
2. Remove the microSD card from Jetson Nano. 4 screws should be removed.
3. Download Qengineering Ubuntu 20.04 Image from https://github.com/Qengineering/Jetson-Nano-Ubuntu-20-image .
4. Download Raspberry Pi Imager from https://github.com/raspberrypi/rpi-imager/releases/latest . It'll be fine with balenaEtcher or other imaging tools.
5. Flash the Ubuntu 20.04 Image to the microSD card using the Raspberry Pi Imager.
6. Insert the microSD card back into the Jetson Nano.
7. Make sure everything is done, you can begin installing ROS2 Foxy.

Tip) your Jetson Nano's password is now "jetson".

#### ROS2 Foxy Installation
```
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update && sudo apt upgrade -y
sudo apt install ros-foxy-desktop python3-argcomplete
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### Git Repository Clone
```
cd ~/
git clone https://github.com/miroo4517/upgrade_limo.git
```

#### Optional, nomachine Installation (for Remote GUI Access Purpose)
```
#optional
sudo dpkg -i nomachine_9.0.188_11_arm64.deb
```

#### udev Port Setting
```
cd ~/upgrade_limo/udev_setting
sudo chmod 777 ./*
./set_port.sh
systemctl stop nvgetty
systemctl disable nvgetty
reboot
```

#### YDLidar SDK Build
```
cd ~/upgrade_limo/YDLidar-SDK/build
cmake ..
make -j4
sudo make install
```

#### YDLidar ROS2 Driver, Limo Package Build
```
mv ~/upgrade_limo/limo_ros2_ws ~/limo_ros2_ws
cd ~/limo_ros2_ws
colcon build
```

#### env variable Setting
```
echo "source ~/limo_ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Yay! If the steps above have been done properly, your Limo ROS1 Robot have been successfully upgraded to ROS2.

