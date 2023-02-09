# This is the method to enable OpenCL support on Jetson Nano

Nvidia does not support OpenCL on Jetson Nano natively. So, the work-around is to install third-party OpenCL library to interface with CUDA, such as [PoCL](http://portablecl.org/). 

## Here is the step to install PoCL on Jetson Nano ( Could work on other Jetson devices as well )

### 1. The PoCL is based on LLVM-Clang compiler framework, so the first step is to install LLVM+Clang. This can be done directly through Ubuntu package management system. 

`export LLVM_VERSION=10`

`sudo apt install -y build-essential ocl-icd-libopencl1 cmake git pkg-config libclang-${LLVM_VERSION}-dev clang-${LLVM_VERSION} llvm-${LLVM_VERSION} make ninja-build ocl-icd-libopencl1 ocl-icd-dev ocl-icd-opencl-dev libhwloc-dev zlib1g zlib1g-dev clinfo dialog apt-utils libxml2-dev libclang-cpp${LLVM_VERSION}-dev libclang-cpp${LLVM_VERSION} llvm-${LLVM_VERSION}-dev libncurses5`

### 2. Download PoCL from its GitHub repository. Here we use the release version 1.7.

`git clone --single-branch --branch release_1_7 https://github.com/pocl/pocl.git`

### 3. Build PoCL on Jetson Nano. Make sure the CMake flag of CUDA backend is enabled. By default, the PoCL will be install in "/usr/local/pocl". You could install to other directories by using CMake Flag "CMAKE_INSTALL_PREFIX".

`cd pocl && mkdir build && cd build`

`cmake -DCMAKE_INSTALL_PREFIX=/usr/local/pocl/ -DENABLE_CUDA=ON ..`

`make && sudo make install`

### 4. Create the Installable Client Driver (ICD) for the PoCL under the folder of "/etc/OpenCL/vendors". If this folder does not exist, need to create first. 

`sudo mkdir /etc/OpenCL/vendors/ && cd /etc/OpenCL/vendors/`

`sudo echo /usr/local/pocl/lib/libpocl.so >> pocl.icd`

### 5. You can install clinfo to check the existing OpenCL devices.

`sudo apt install clinfo`

`clinfo`

#### This is what it shows on my Jetson Nano: 

<img src="https://github.com/wincle626/JetsonNano_Setup/blob/main/pics/clinfo.png">

Number of platforms                               1

  Platform Name                                   Portable Computing Language
  
  Platform Vendor                                 The pocl project
  
  Platform Version                                OpenCL 2.0 pocl 1.7, Debug+Asserts, LLVM 10.0.0, RELOC, SLEEF, FP16, CUDA, POCL_DEBUG
  
  Platform Profile                                FULL_PROFILE
  
  Platform Extensions                             cl_khr_icd
  
  Platform Extensions function suffix             POCL

  Platform Name                                   Portable Computing Language
  
Number of devices                                 2

  Device Name                                     pthread-cortex-a57
  
  Device Vendor                                   ARM
  
  Device Vendor ID                                0x13b5
  
  Device Version                                  OpenCL 1.2 pocl HSTR: pthread-aarch64-unknown-linux-gnu-cortex-a57
  
  Driver Version                                  1.7
  
  Device OpenCL C Version                         OpenCL C 1.2 pocl
  
  Device Type                                     CPU
  
  Device Profile                                  FULL_PROFILE
  
  Device Available                                Yes
  
  Compiler Available                              Yes
  
  Linker Available                                Yes
  
  Max compute units                               4
  
  Max clock frequency                             1479MHz
  
  Device Partition                                (core)
  
    Max number of sub-devices                     4
    
    Supported partition types                     equally, by counts
    
  Max work item dimensions                        3
  
  Max work item sizes                             4096x4096x4096
  
  Max work group size                             4096
  
  Preferred work group size multiple              8
  
  Preferred / native vector sizes                 
    char                                                16 / 16   
    
    short                                                8 / 8    
    
    int                                                  4 / 4      
    
    long                                                 2 / 2     
    
    half                                                 8 / 8        (cl_khr_fp16)
    
    float                                                4 / 4       
    
    double                                               2 / 2        (cl_khr_fp64)
    
  Half-precision Floating-point support           (cl_khr_fp16)
  
    Denormals                                     No
    
    Infinity and NANs                             No
    
    Round to nearest                              No
    
    Round to zero                                 No
    
    Round to infinity                             No
    
    IEEE754-2008 fused multiply-add               No
    
    Support is emulated in software               No
    
  Single-precision Floating-point support         (core)
  
    Denormals                                     No
    
    Infinity and NANs                             Yes
    
    Round to nearest                              Yes
    
    Round to zero                                 No
    
    Round to infinity                             No
    
    IEEE754-2008 fused multiply-add               No
    
    Support is emulated in software               No
    
    Correctly-rounded divide and sqrt operations  No
    
  Double-precision Floating-point support         (cl_khr_fp64)
  
    Denormals                                     Yes
    
    Infinity and NANs                             Yes
    
    Round to nearest                              Yes
    
    Round to zero                                 Yes
    
    Round to infinity                             Yes
    
    IEEE754-2008 fused multiply-add               Yes
    
    Support is emulated in software               No
    
  Address bits                                    64, Little-Endian
  
  Global memory size                              3117428736 (2.903GiB)
  
  Error Correction support                        No
  
  Max memory allocation                           1073741824 (1024MiB)
  
  Unified memory for Host and Device              Yes
  
  Minimum alignment for any data type             128 bytes
  
  Alignment of base address                       1024 bits (128 bytes)
  
  Global Memory cache type                        Read/Write
  
  Global Memory cache size                        2097152 (2MiB)
  
  Global Memory cache line size                   64 bytes
  
  Image support                                   Yes
  
    Max number of samplers per kernel             16
    
    Max size for 1D images from buffer            67108864 pixels
    
    Max 1D or 2D image array size                 2048 images
    
    Max 2D image size                             8192x8192 pixels
    
    Max 3D image size                             2048x2048x2048 pixels
    
    Max number of read image args                 128
    
    Max number of write image args                128
    
  Local memory type                               Global
  
  Local memory size                               32768 (32KiB)
  
  Max number of constant args                     8
  
  Max constant buffer size                        32768 (32KiB)
  
  Max size of kernel argument                     1024
  
  Queue properties                                
  
    Out-of-order execution                        Yes
    
    Profiling                                     Yes
    
  Prefer user sync for interop                    Yes
  
  Profiling timer resolution                      1ns
  
  Execution capabilities                          
  
    Run OpenCL kernels                            Yes
    
    Run native kernels                            Yes
    
  printf() buffer size                            16777216 (16MiB)
  
  Built-in kernels                                
  
  Device Extensions                               cl_khr_byte_addressable_store cl_khr_global_int32_base_atomics cl_khr_global_int32_extended_atomics cl_khr_local_int32_base_atomics cl_khr_local_int32_extended_atomics cl_khr_3d_image_writes cl_khr_fp16 cl_khr_fp64
  

  Device Name                                     NVIDIA Tegra X1
  
  Device Vendor                                   NVIDIA Corporation
  
  Device Vendor ID                                0x10de
  
  Device Version                                  OpenCL 1.2 pocl HSTR: CUDA-sm_53
  
  Driver Version                                  1.7
  
  Device OpenCL C Version                         OpenCL C 1.2 pocl
  
  Device Type                                     GPU
  
  Device Topology (NV)                            PCI-E, 00:00.0
  
  Device Profile                                  FULL_PROFILE
  
  Device Available                                Yes
  
  Compiler Available                              Yes
  
  Linker Available                                Yes
  
  Max compute units                               1
  
  Max clock frequency                             921MHz
  
  Compute Capability (NV)                         5.3
  
  Device Partition                                (core)
  
    Max number of sub-devices                     1
    
    Supported partition types                     None
    
  Max work item dimensions                        3
  
  Max work item sizes                             1024x1024x64
  
  Max work group size                             1024
  
  Preferred work group size multiple              32
  
  Warp size (NV)                                  32
  
  Preferred / native vector sizes                 
  
    char                                                 1 / 1    
    
    short                                                1 / 1   
    
    int                                                  1 / 1   
    
    long                                                 1 / 1   
    
    half                                                 0 / 0        (n/a)
    
    float                                                1 / 1       
    
    double                                               1 / 1        (cl_khr_fp64)
    
  Half-precision Floating-point support           (n/a)
  
  Single-precision Floating-point support         (core)
  
    Denormals                                     Yes
    
    Infinity and NANs                             Yes
    
    Round to nearest                              Yes
    
    Round to zero                                 Yes
    
    Round to infinity                             Yes
    
    IEEE754-2008 fused multiply-add               Yes
    
    Support is emulated in software               No
    
    Correctly-rounded divide and sqrt operations  No
    
  Double-precision Floating-point support         (cl_khr_fp64)
  
    Denormals                                     Yes
    
    Infinity and NANs                             Yes
    
    Round to nearest                              Yes
    
    Round to zero                                 Yes
    
    Round to infinity                             Yes
    
    IEEE754-2008 fused multiply-add               Yes
    
    Support is emulated in software               No
    
  Address bits                                    64, Little-Endian
  
  Global memory size                              4156571648 (3.871GiB)
  
  Error Correction support                        No
  
  Max memory allocation                           1039142912 (991MiB)
  
  Unified memory for Host and Device              Yes
  
  Integrated memory (NV)                          Yes
  
  Minimum alignment for any data type             128 bytes
  
  Alignment of base address                       4096 bits (512 bytes)
  
  Global Memory cache type                        None
  
  Image support                                   No
  
  Local memory type                               Local
  
  Local memory size                               49152 (48KiB)
  
  Registers per block (NV)                        32768
  
  Max number of constant args                     8
  
  Max constant buffer size                        65536 (64KiB)
  
  Max size of kernel argument                     1024
  
  Queue properties                                
  
    Out-of-order execution                        No
    
    Profiling                                     Yes
    
  Prefer user sync for interop                    Yes
  
  Profiling timer resolution                      1ns
  
  Execution capabilities                          
  
    Run OpenCL kernels                            Yes
    
    Run native kernels                            No
    
    Kernel execution timeout (NV)                 Yes
    
  Concurrent copy and kernel execution (NV)       Yes
  
    Number of async copy engines                  1
    
  printf() buffer size                            16777216 (16MiB)
  
  Built-in kernels                                
  
  Device Extensions                               cl_khr_byte_addressable_store cl_khr_global_int32_base_atomics cl_khr_global_int32_extended_atomics cl_khr_local_int32_base_atomics cl_khr_local_int32_extended_atomics cl_khr_fp64 cl_khr_int64_base_atomics cl_khr_int64_extended_atomics cl_nv_device_attribute_query
  

NULL platform behavior

  clGetPlatformInfo(NULL, CL_PLATFORM_NAME, ...)  Portable Computing Language
  
  clGetDeviceIDs(NULL, CL_DEVICE_TYPE_ALL, ...)   Success [POCL]
  
  clCreateContext(NULL, ...) [default]            Success [POCL]
  
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_DEFAULT)  Success (1)
  
    Platform Name                                 Portable Computing Language
    
    Device Name                                   pthread-cortex-a57
    
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_CPU)  Success (1)
  
    Platform Name                                 Portable Computing Language
    
    Device Name                                   pthread-cortex-a57
    
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_GPU)  Success (1)
  
    Platform Name                                 Portable Computing Language
    
    Device Name                                   NVIDIA Tegra X1
    
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_ACCELERATOR)  No devices found in platform
  
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_CUSTOM)  No devices found in platform
  
  clCreateContextFromType(NULL, CL_DEVICE_TYPE_ALL)  Success (2)
  
    Platform Name                                 Portable Computing Language
    
    Device Name                                   pthread-cortex-a57
    
    Device Name                                   NVIDIA Tegra X1
    

ICD loader properties

  ICD loader Name                                 OpenCL ICD Loader
  
  ICD loader Vendor                               OCL Icd free software
  
  ICD loader Version                              2.2.11
  
  ICD loader Profile                              OpenCL 2.1
  




