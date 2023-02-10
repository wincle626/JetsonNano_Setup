# There is detailed steps on [ROS.org](http://wiki.ros.org/). Here just record the important steps. 

## 1. Due to the Ubuntu version of Jetson Nano image is 18.04. The [Melodic](http://wiki.ros.org/ROS/Installation) will be considered using [Ubuntu installation option](http://wiki.ros.org/melodic/Installation/Ubuntu). 

## 2. Setup ROS repository in sources.list 

Setup Jetson Nano to accept software from packages.ros.org.

  `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

  `sudo apt install curl # if you haven't already installed curl`

  `curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

## 3. Install Melodic

  `sudo apt update`

  `sudo apt install ros-melodic-desktop-full`

## 4. Setup ROS enviroment

  `echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc && source ~/.bashrc`

## 5. In case of building ROS package in the future, you need to install necessory Ubuntu package and setup rosdep

  `sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential`

  `sudo rosdep init && rosdep update`

## 6. Now it is time to play ROS on Jetson Nano :)

### a. Create a ROS Workspace

  `mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/ && catkin_make`

(if Python 3 is require for deveropment, then use "catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3" to create the workspace)

### b. Setup the devleopment enviroment

  `source devel/setup.bash`

make sure "/home/youruser/catkin_ws/src" is added to ROS_PACKAGE_PATH and can be checked by 

  `echo $ROS_PACKAGE_PATH`
  
### c. Toy ROS project

#### i. Creating a catkin Package using ROS example

go to the directory of the catkin workspace:

  `cd ~/catkin_ws/src`

use the catkin_create_pkg script to create a new package called 'beginner_tutorials' which depends on std_msgs, roscpp, and rospy:

  `catkin_create_pkg beginner_tutorials std_msgs rospy roscpp`

(usage of catkin_create_pkg script with a list of dependencies: catkin_create_pkg <package_name> [depend1] [depend2] [depend3] ... )

#### ii. Building a catkin workspace and sourcing the setup file

  `cd ~/catkin_ws && $ catkin_make`

  `source ~/catkin_ws/devel/setup.bash` (you can also put it into .bashrc)

#### iii. Check package dependencies

first-order dependencies can now be reviewed with the rospack tool:

  `rospack depends1 beginner_tutorials `

these dependencies for a package are stored in the package.xml file:

  `roscd beginner_tutorials && cat package.xml`
  
  <package format="2">
    ...
      <buildtool_depend>catkin</buildtool_depend>
      <build_depend>roscpp</build_depend>
      <build_depend>rospy</build_depend>
      <build_depend>std_msgs</build_depend>
    ...
  </package>

 dependency will also have its own dependencies, which can be checked as well:
 
  `rospack depends1 rospy`
  
rospack can recursively determine all nested dependencies:
 
  `rospack depends beginner_tutorials`
  
#### iii. Customizing the package

the meta information of package in [`package.xml`](http://wiki.ros.org/catkin/package.xml) can be changed to customize the contact, author, licence, dependencies, and version, etc.

e.g. 
   1 <?xml version="1.0"?>
   2 <package format="2">
   3   <name>beginner_tutorials</name>
   4   <version>0.1.0</version>
   5   <description>The beginner_tutorials package</description>
   6 
   7   <maintainer email="you@yourdomain.tld">Your Name</maintainer>
   8   <license>BSD</license>
   9   <url type="website">http://wiki.ros.org/beginner_tutorials</url>
  10   <author email="you@yourdomain.tld">Jane Doe</author>
  11 
  12   <buildtool_depend>catkin</buildtool_depend>
  13 
  14   <build_depend>roscpp</build_depend>
  15   <build_depend>rospy</build_depend>
  16   <build_depend>std_msgs</build_depend>
  17 
  18   <exec_depend>roscpp</exec_depend>
  19   <exec_depend>rospy</exec_depend>
  20   <exec_depend>std_msgs</exec_depend>
  21 
  22 </package>
  



