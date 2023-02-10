# How to build and install OpenCV on Jetson Nano 

Following the procedures by [JetsonHack's repository](https://github.com/JetsonHacksNano/buildOpenCV):

## 1. Download the latest OpenCV version 4.7.0.

  `wget https://github.com/opencv/opencv/archive/refs/tags/4.7.0.zip && unzip 4.7.0.zip`
  
## 2. Install prerequisite packages

sudo apt-add-repository universe
sudo apt-get update
sudo apt-get install -y \
    build-essential \
    cmake \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libeigen3-dev \
    libglew-dev \
    libgtk2.0-dev \
    libgtk-3-dev \
    libjpeg-dev \
    libpng-dev \
    libpostproc-dev \
    libswscale-dev \
    libtbb-dev \
    libtiff5-dev \
    libv4l-dev \
    libxvidcore-dev \
    libx264-dev \
    qt5-default \
    zlib1g-dev \
    pkg-config
