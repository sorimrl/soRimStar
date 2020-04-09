# soRimStar
This is private git repository for Studying Reinforcement Learning with StarCraft.

# SC1

https://torchcraft.github.io/TorchCraftAI/

## Installation guide

### 0. clone torchAI
### git clone https://github.com/TorchCraft/TorchCraftAI --recursive


### 1. Installation cuda9.2 && nccl PATH setting
### https://developer.nvidia.com/cuda-92-download-archive
### https://medium.com/@asmello/how-to-install-tensorflow-cuda-9-1-into-ubuntu-18-04-b645e769f01d

### 2. Installation Following packages
### sudo apt-get install git libsdl2-dev libzmq3-dev binutils-dev libdw-dev libgflags-dev libnuma-dev cmake libgoogle-glog-dev

### 3. Building Anaconda Env. and installing following packages
### conda create --name starcraft python=3.6
### export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
### conda install numpy pyyaml mkl=2019.1 mkl-include=2019.1 setuptools cmake cffi typing
### conda install -c mingfeima mkldnn

### 4. Building Torch in TorchCraftAI
### pushd 3rdparty/pytorch/tools/
### REL_WITH_DEB_INFO=1 python build_libtorch.py




# SC2

https://github.com/deepmind/pysc2#get-starcraft-ii

## Development Env

### Ubuntu18.04
### Anaconda3
### pysc2
### sc2 ver. 4.10 for linux - very recent ver. 2020.03.30 


