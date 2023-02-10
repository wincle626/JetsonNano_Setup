# How to build and install OpenCV on Jetson Nano 

Following the procedures by [JetsonHack's repository](https://github.com/JetsonHacksNano/buildOpenCV):

## 1. Download the latest OpenCV version 4.7.0.

  `wget https://github.com/opencv/opencv/archive/refs/tags/4.7.0.zip && unzip 4.7.0.zip`
  
  `wget https://github.com/opencv/opencv_contrib/archive/refs/tags/4.7.0.zip`
  
using `unzip` command to decompress the zip file to home directory. 
  
## 2. Install prerequisite packages

`sudo apt-add-repository universe`

`sudo apt update`

`sudo apt install -y build-essential cmake libavcodec-dev libavformat-dev libavutil-dev libeigen3-dev libglew-dev libgtk2.0-dev libgtk-3-dev libjpeg-dev libpng-dev libpostproc-dev libswscale-dev libtbb-dev libtiff5-dev libv4l-dev libxvidcore-dev libx264-dev qt5-default zlib1g-dev pkg-config`

`sudo apt-get install -y python-dev  python-numpy  python-py  python-pytest`

`sudo apt-get install -y python3-dev python3-numpy python3-py python3-pytest`

`sudo apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev `

## 3. Build OpenCV using CMake

cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=~/opencv-4.7.0/install -DWITH_CUDA=ON -DCUDA_ARCH_BIN=5.3 -DCUDA_ARCH_PTX="" -DENABLE_FAST_MATH=ON -DCUDA_FAST_MATH=ON -DWITH_CUBLAS=ON -DWITH_LIBV4L=ON -DWITH_V4L=ON -DWITH_GSTREAMER=ON -DWITH_GSTREAMER_0_10=OFF -DWITH_QT=ON -DWITH_OPENGL=ON -DBUILD_opencv_python2=ON -DBUILD_opencv_python3=ON -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.7.0/modules -DCPACK_BINARY_DEB=ON ../
