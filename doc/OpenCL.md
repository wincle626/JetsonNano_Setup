# This is the method to enable OpenCL support on Jetson Nano

Nvidia does not support OpenCL on Jetson Nano natively. So, the work-around is to install third-party OpenCL library to interface with CUDA, such as [PoCL](http://portablecl.org/). 

## Here is the step to install PoCL on Jetson Nano ( Could work on other Jetson devices as well )

### 1. The PoCL is based on LLVM-Clang compiler framework, so the first step is to install LLVM+Clang. This can be done directly through Ubuntu package management system. 

export LLVM_VERSION=10

sudo apt install -y build-essential ocl-icd-libopencl1 cmake git pkg-config libclang-${LLVM_VERSION}-dev clang-${LLVM_VERSION} llvm-${LLVM_VERSION} make ninja-build ocl-icd-libopencl1 ocl-icd-dev ocl-icd-opencl-dev libhwloc-dev zlib1g zlib1g-dev clinfo dialog apt-utils libxml2-dev libclang-cpp${LLVM_VERSION}-dev libclang-cpp${LLVM_VERSION} llvm-${LLVM_VERSION}-dev libncurses5
