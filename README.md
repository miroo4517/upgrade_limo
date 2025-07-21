```
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt upgrade
sudo apt install ros-foxy-desktop python3-argcomplete
source /opt/ros/foxy/setup.bash
```

```
cd ~/
git clone https://github.com/WeGo-Robotics/upgrade_limo.git
```

```
cd ~/upgrade_limo/system_setting
sudo dpkg -i nomachine_9.0.188_11_arm64.deb
```

```
cd ~/upgrade_limo/system_setting/udev_setting
sudo chmod 777 ./*
./set_port.sh
systemctl stop nvgetty
systemctl disable nvgetty
reboot
```

```
cd ~/upgrade_limo/YDLidar-SDK
mkdir build
cd build
cmake ..
make -j4
sudo make install
```

```
mkdir -p ~/ros2_ws/src
cd ~/upgrade_limo/
mv limo_ros2 ~/ros2_ws/src/
mv ydlidar_ros2_driver ~/ros2_ws/src/
```

```
cd ~/ros2_ws
colcon build
```

```
source install/setup.bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```
