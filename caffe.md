# Installing Caffe on Ubuntu 16.04 with CUDA 8 and CuDNN support

First Set up Nvidia Drivers, CUDA 8, CuDNN and OpenCV (optional) with Ubuntu 16.04. You can follow these [instructions](https://github.com/sakthi-s/Installation-Instructions/blob/master/setup_cuda_cudnn_opencv3.md).

Once you've successfully completely the above steps, you can move on to building Caffe.

1. Install the required dependencies
```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y opencl-headers build-essential protobuf-compiler \
    libprotoc-dev libboost-all-dev libleveldb-dev hdf5-tools libhdf5-serial-dev \
    libopencv-core-dev  libopencv-highgui-dev libsnappy-dev \
    libatlas-base-dev cmake libstdc++6-4.8-dbg libgoogle-glog0v5 libgoogle-glog-dev \
    libgflags-dev liblmdb-dev git python-pip gfortran libopencv-dev
sudo apt-get clean
```

2. Get Caffe and install the python requirements
```
git clone https://github.com/BVLC/caffe.git
cd caffe
cd python
for req in $(cat requirements.txt); do sudo pip install $req; done
```

3. Configure Makefile
```
# Rename the file and edit the renamed file
cp Makefile.config.example Makefile.config
gedit Makefile.config
```

Make the following changes:
  + Uncomment: `USE_CUDNN := 1`
  + Uncomment: `OPENCV_VERSION := 3` (If you've installed OpenCV 3)
  + Modify: `CUDA_DIR := /usr/local/cuda` to `CUDA_DIR := /usr/local/cuda-8.0`
  + Modify: `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include` to `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial`
  + Modify: `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib` to `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial`
  + Uncomment: `WITH_PYTHON_LAYER := 1`

__NOTE__: The instructions are for default python interpreter. If you would like to set up Caffe for Anaconda, you'll need to make appropriate modifications to the `Makefile.config`.

4. `libhdf5` Linking
 Visit `/usr/lib/x86_64-linux-gnu/` and list the relevant contents of that directory by using the command such as `ls -l | grep hdf5`. The versions of libhdf5 that need to be linked to are 10.1.0 and 10.0.2 respectively:
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libhdf5_serial.so.10.1.0 /usr/lib/x86_64-linux-gnu/libhdf5.so
sudo ln -s /usr/lib/x86_64-linux-gnu/libhdf5_serial_hl.so.10.0.2 /usr/lib/x86_64-linux-gnu/libhdf5_hl.so
```

5. Build Caffe
```
make -j 8 all py
make -j 8 test
make runtest
```

6. Set up Environment Variables
Open bashrc file and add the following: 
```
export PYTHONPATH=/path/to/caffe-master/python:$PYTHONPATH
```
