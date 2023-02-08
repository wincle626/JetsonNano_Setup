# This is the method to enable OpenCL support on Jetson Nano

Nvidia does not support OpenCL on Jetson Nano natively. So, the work-around is to install third-party OpenCL library to interface with CUDA, such as [PoCL](http://portablecl.org/). 

## Here is the step to install PoCL on Jetson Nano ( Could work on other Jetson devices as well )

### 1. The PoCL is based on LLVM-Clang compiler framework, so the first step is to install LLVM+Clang. This can be done directly through Ubuntu package management system. 

export LLVM_VERSION=10

sudo apt install -y build-essential ocl-icd-libopencl1 cmake git pkg-config libclang-${LLVM_VERSION}-dev clang-${LLVM_VERSION} llvm-${LLVM_VERSION} make ninja-build ocl-icd-libopencl1 ocl-icd-dev ocl-icd-opencl-dev libhwloc-dev zlib1g zlib1g-dev clinfo dialog apt-utils libxml2-dev libclang-cpp${LLVM_VERSION}-dev libclang-cpp${LLVM_VERSION} llvm-${LLVM_VERSION}-dev libncurses5

### 2. Download PoCL from its GitHub repository. Here we use the release version 1.7.

git clone --single-branch --branch release_1_7 https://github.com/pocl/pocl.git

### 3. Build PoCL on Jetson Nano. Make sure the CMake flag of CUDA backend is enabled. By default, the PoCL will be install in "/usr/local/pocl". You could install to other directories by using CMake Flag "CMAKE_INSTALL_PREFIX".

cd pocl && mkdir build && cd build

cmake -DCMAKE_INSTALL_PREFIX=/usr/local/pocl/ -DENABLE_CUDA=ON ..

make && sudo make install

### 4. Create the Installable Client Driver (ICD) for the PoCL under the folder of "/etc/OpenCL/vendors". If this folder does not exist, need to create first. 

sudo mkdir /etc/OpenCL/vendors/ && cd /etc/OpenCL/vendors/

sudo echo /usr/local/pocl/lib/libpocl.so >> pocl.icd

### 5. You can install clinfo to check the existing OpenCL devices.

sudo apt install clinfo

clinfo






