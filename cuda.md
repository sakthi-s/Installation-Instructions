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
{LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

Verify Installation by installing CUDA samples:
```
cuda-install-samples-8.0.sh ~
cd ~/NVIDIA_CUDA-8.0_Samples/5_Simulations/nbody
make
./nbody
```


## OpenCV 3
Use the following link to install OpenCV 3: [https://github.com/BVLC/caffe/wiki/OpenCV-3.2-Installation-Guide-on-Ubuntu-16.04](https://github.com/BVLC/caffe/wiki/OpenCV-3.2-Installation-Guide-on-Ubuntu-16.04)

## 
