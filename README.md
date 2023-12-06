# Install-Opencv-instruction
install opencv with/without CUDA support on Ubuntu

## 
# How to install OpenCV 4.2.0 with CUDA 11.8 in Ubuntu 20.04

First of all, try to update and upgrade your system:
    
        $ sudo apt update
        $ sudo apt upgrade
   
    
Then, install required libraries:

* Generic tools:

        $ sudo apt install build-essential cmake pkg-config unzip yasm git checkinstall
    
* Image I/O libs
    ``` 
    $ sudo apt install libjpeg-dev libpng-dev libtiff-dev
    ``` 
* Video/Audio Libs - FFMPEG, GSTREAMER, x264 and so on.
    ```
    $ sudo apt install libavcodec-dev libavformat-dev libswscale-dev libavresample-dev
    $ sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
    $ sudo apt install libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev 
    $ sudo apt install libfaac-dev libmp3lame-dev libvorbis-dev
    ```
* OpenCore - Adaptive Multi Rate Narrow Band (AMRNB) and Wide Band (AMRWB) speech codec
    ```
    $ sudo apt install libopencore-amrnb-dev libopencore-amrwb-dev
    ```
    
* Cameras programming interface libs
    ```
    $ sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
    $ cd /usr/include/linux
    $ sudo ln -s -f ../libv4l1-videodev.h videodev.h
    $ cd ~
    ```

* GTK lib for the graphical user functionalites coming from OpenCV highghui module 
    ```
    $ sudo apt-get install libgtk-3-dev
    ```
* Python libraries for python3:
    ```
    $ sudo apt-get install python3-dev python3-pip
    $ sudo -H pip3 install -U pip numpy
    $ sudo apt install python3-testresources
    ```
* Parallelism library C++ for CPU
    ```
    $ sudo apt-get install libtbb-dev
    ```
* Optimization libraries for OpenCV
    ```
    $ sudo apt-get install libatlas-base-dev gfortran
    ```
* Optional libraries:
    ```
    $ sudo apt-get install libprotobuf-dev protobuf-compiler
    $ sudo apt-get install libgoogle-glog-dev libgflags-dev
    $ sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen
    ```

Check CUDA installed.
    ```
    $ nvidia-smi
    $ nvcc --version
    ```

We will now proceed with the installation.
Download the expected version of [OpenCV](https://github.com/opencv/opencv/tags) and OpenCV-contrib[https://github.com/opencv/opencv_contrib](https://github.com/opencv/opencv_contrib/tags).
Please make sure that the version of these must be same.


    $ cd ~/Downloads
    $ unzip opencv.zip
    $ unzip opencv_contrib.zip
    
    $ cd opencv-4.2.0
    $ mkdir build
    $ cd build

Change the OPENCV_EXTRA_MODULES_PATH if your opencv_contrib directory is not the same name.
Change WITH_CUDA WITH_CUDNN to make sure you nedd CUDA and Cudnn support or not.
*-D CUDA_ARCH_BIN* is based on your [GPU Compute Capability](https://developer.nvidia.com/cuda-gpus).
In case you do not want to include include CUDA set *-D WITH_CUDA=OFF*  
   
    cmake     -D CMAKE_BUILD_TYPE=RELEASE   
    -D CMAKE_C_COMPILER=/usr/bin/gcc     
    -D CMAKE_INSTALL_PREFIX=/usr/local     
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/     
    -D CUDA_CUDA_LIBRARY=/usr/local/cuda/lib64/stubs/libcuda.so     
    -D CUDA_ARCH_BIN=7.0     
    -D CUDA_ARCH_PTX=""     
    -D WITH_CUDA=ON     
    -D WITH_TBB=ON     
    -D WITH_FFMPEG=ON     
    -D WITH_V4L=ON     
    -D INSTALL_C_EXAMPLES=ON     
    -D BUILD_EXAMPLES=ON     
    -D WITH_QT=ON     
    -D WITH_GSTREAMER=ON     
    -D WITH_OPENGL=ON     
    -D ENABLE_FAST_MATH=1     
    -D CUDA_FAST_MATH=1     
    -D OPENCV_GENERATE_PKGCONFIG=ON     
    -D OPENCV_PC_FILE_NAME=opencv.pc     
    -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda     
    -D CMAKE_LIBRARY_PATH=/usr/local/cuda/lib64/stubs     
    -D WITH_CUBLAS=ON     
    -D WITH_NVCUVID=ON     
    -D BUILD_opencv_cudacodec=OFF     
    -D OPENCV_DNN_CUDA=OFF     
    -D WITH_CUDNN=OFF     
    -D OPENCV_ENABLE_NONFREE=ON    
    -D WITH_GSTREAMER=ON     
    -D BUILD_EXAMPLES=ON ..


Before the compilation you must check that CUDA has been enabled in the configuration summary printed on the screen. (If you have problems with the CUDA Architecture go to the end of the document).

```
--   NVIDIA CUDA:                   YES (ver 11.2, CUFFT CUBLAS FAST_MATH)
--     NVIDIA GPU arch:             75
--     NVIDIA PTX archs:
-- 
--   cuDNN:                         YES (ver 8.2.0)

```

If it is fine proceed with the compilation (Use nproc to know the number of cpu cores):
    
    $ nproc
    $ make -j$(nproc)
    $ sudo make install
