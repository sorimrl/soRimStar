# Docker is Ready

docker push woodolee/torchcraftai:latest


# How to set docker with cuda9.2 & ubuntu18.04 to study torchcraftai

sudo docker run --gpus all -t -d --name torchcraft_gpu_on nvidia/cuda:9.2-devel-ubuntu18.04

## apt package & conda env

sudo docker exec -it torchcraft_gpu_on bash

apt-get install git libsdl2-dev libzmq3-dev binutils-dev libdw-dev libgflags-dev libnuma-dev cmake curl libgoogle-glog-dev wget libhiredis-dev

wget https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh

chmod +x Anaconda3-2020.02-Linux-x86_64.sh 

./Anaconda3-2020.02-Linux-x86_64.sh 

conda create --name torchcraft python=3.6

conda activate torchcraft

conda install numpy pyyaml mkl=2019.1 mkl-include=2019.1 setuptools cmake cffi typing

conda install -c mingfeima mkldnn

## push docker conda

sudo docker commit -m "[commit description]" -a "[dockerHubID]" [containerID] [yourRepository:tag]

sudo docker push woodolee/torchcraftai

## torchcraft building
sudo docker exec -it [container Name] bash

conda activate torchcraft

conda update -n base -c defaults conda

git clone https://github.com/TorchCraft/TorchCraftAI --recursive

cd TorchCraft

export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
export TORCH_CUDA_ARCH_LIST="7.0"

pushd 3rdparty/pytorch/tools/

REL_WITH_DEB_INFO=1 python build_libtorch.py

exit

## push docker torchcraftai

sudo docker commit -m "[commit description]" -a "[dockerHubID]" [containerID] [yourRepository:tag]

sudo docker push [yourRepository:tag]

## zstandard

sudo docker exec -it [container Name] bash

curl -sSL https://github.com/facebook/zstd/archive/v1.3.3.tar.gz | tar xvzf -
pushd zstd-1.3.3/build/cmake
cmake . -DCMAKE_C_FLAGS=-fPIC -DCMAKE_CXX_FLAGS=-fPIC -DZSTD_BUILD_STATIC=ON -DCMAKE_BUILD_TYPE=Release -DZSTD_LEGACY_SUPPORT=0
make -j$(nproc)
make install
popd

## OpenBW

pushd 3rdparty/openbw
mkdir -p build
cd build
cmake .. -DCMAKE_BUILD_TYPE=release -DOPENBW_ENABLE_UI=1
make -j$(nproc)
make install
popd

## starcraft1161
wget http://www.cs.mun.ca/~dchurchill/starcraftaicomp/files/Starcraft_1161.zip
mkdir Starcraft1161
cd Starcraft1161
unzip -d Starcraft_1161.zip ./Starcraft1161

## wine
dpkg --add-architecture i386
apt update
apt install software-properties-common
apt install --install-recommends winehq-stable
apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main'
add-apt-repository ppa:cybermax-dexter/sdl2-backport
apt update && apt install --install-recommends winehq-stable
winecfg
mv ~/Starcraft1161 ~/.wine

## Build Cherrypi

conda install -c anaconda libcurl
cd ~/TorchCraftAI
mkdir build 
cmake .. -DCMAKE_BUILD_TYPE=relwithdebinfo

### Debug modifying code
vim ~/root/TorchCraftAI/tutorials/micro/micro_tutorial
:402
cherrypi::ForkServer::startForkServer();
make -j4
exit

### docker pushing Cherrypi
sudo docker commit -m "[commit description]" -a "[dockerHubID]" [containerID] [yourRepository:tag]
sudo docker push [yourRepository:tag]

## Run tutorials

sudo docker run --gpus all --name [New container Name] -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix woodolee/torchcraftai:latest /bin/bash

sudo docker exec -it [New container Name] bash

### inside docker

cd ~/TorchCraftAI
./build/tutorials/micro/micro_tutorial -vmodule state=-1,micro_tutorial=1 -scenario vu_zl




