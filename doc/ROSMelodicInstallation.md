# There is detailed steps on [ROS.org](http://wiki.ros.org/). Here just record the important steps. 

## 1. Due to the Ubuntu version of Jetson Nano image is 18.04. The [Melodic](http://wiki.ros.org/ROS/Installation) will be considered using [Ubuntu installation option](http://wiki.ros.org/melodic/Installation/Ubuntu). 

## 2. Setup ROS repository in sources.list 

Setup Jetson Nano to accept software from packages.ros.org.

  sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

  sudo apt install curl # if you haven't already installed curl

  curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

## 3. Install Melodic

  sudo apt update

  sudo apt install ros-melodic-desktop-full

## 4. Setup ROS enviroment

  echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc && source ~/.bashrc

## 5. In case of building ROS package in the future, you need to install necessory Ubuntu package and setup rosdep

  sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

  sudo rosdep init && rosdep update

## 6. Now it is time to play ROS on Jetson Nano :)

### a. Create a ROS Workspace

  mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/ && catkin_make

(if Python 3 is require for deveropment, then use "catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3" to create the workspace)

### b. Setup the devleopment enviroment

  `source devel/setup.bash`

make sure "/home/youruser/catkin_ws/src" is added to ROS_PACKAGE_PATH and can be checked by 

  `echo $ROS_PACKAGE_PATH`
