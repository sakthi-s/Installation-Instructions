# CUDA, CuDNN and OpenCV 3 with Ubuntu 16.04


## Install the necessary pre-requisites

```shell

sudo apt-get update

sudo apt-get upgrade

sudo apt-get install -y build-essential cmake git pkg-config

sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler

sudo apt-get install -y libatlas-base-dev 

sudo apt-get install -y --no-install-recommends libboost-all-dev

sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev

# (Python general)
sudo apt-get install -y python-pip

# (Python 2.7 development files)
sudo apt-get install -y python-dev
sudo apt-get install -y python-numpy python-scipy

# (or, Python 3.5 development files)
sudo apt-get install -y python3-dev
sudo apt-get install -y python3-numpy python3-scipy

```


## Install Anaconda (Optional)

Download and install Anaconda with appropriate python version. 
[https://www.continuum.io/downloads](https://www.continuum.io/downloads)


## Install CUDA

Download CUDA Toolkit for your system configuration (download the `deb` file)
[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

Follow these commands to install CUDA
```Shell
sudo dpkg -i path/to/cuda
sudo apt-get update
sudo apt-get install cuda
```

Add path to CUDA and CUDA libraries to bashrc (environment variables)
`gedit ~/.bashrc`
Add these lines to bashrc file
```
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}} 
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64\
${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

Verify Installation by installing CUDA samples:
```
cuda-install-samples-8.0.sh ~
cd ~/NVIDIA_CUDA-8.0_Samples/5_Simulations/nbody
make
./nbody
```


## CuDNN 

Download CuDNN (You might have to creat a login) [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)
Extract the files and copy the contents of `include` and `lib64` directories into the respective `cuda` folder in `/usr/local/`.
You can refer to these commands.
```
sudo cp lib64/* /usr/local/cuda/lib64
sudo cp include/* /usr/local/cuda/include
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## OpenCV 3.1

The below instructions are modified from [https://github.com/BVLC/caffe/wiki/OpenCV-3.2-Installation-Guide-on-Ubuntu-16.04](https://github.com/BVLC/caffe/wiki/OpenCV-3.2-Installation-Guide-on-Ubuntu-16.04) as it didn't work for me with CUDA 8.0

Clone the OpenCV repository and checkout version 3.1.0-with-cuda. 
```
git clone https://github.com/daveselinger/opencv.git
git checkout 3.1.0-with-cuda8
```

Install the required dependencies. 
```
sudo apt-get install --assume-yes build-essential cmake git
sudo apt-get install --assume-yes pkg-config unzip ffmpeg qtbase5-dev python-dev python3-dev python-numpy python3-numpy
sudo apt-get install --assume-yes libopencv-dev libgtk-3-dev libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev
sudo apt-get install --assume-yes libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
sudo apt-get install --assume-yes libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev
sudo apt-get install --assume-yes libvorbis-dev libxvidcore-dev v4l-utils python-vtk
sudo apt-get install --assume-yes liblapacke-dev libopenblas-dev checkinstall
sudo apt-get install --assume-yes libgdal-dev
```

Build
```
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_CUDA=ON -D WITH_CUBLAS=ON -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D BUILD_PERF_TESTS=OFF -D BUILD_TESTS=OFF -DCUDA_NVCC_FLAGS="-D_FORCE_INLINES" ..
make -j $(($(nproc) + 1))
sudo make install
```
