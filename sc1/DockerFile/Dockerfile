# We will use Ubuntu for our image
FROM ubuntu:latest

# Updating Ubuntu packages
RUN apt-get update && yes|apt-get upgrade
RUN apt-get install -y emacs

#Cuda
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility

# Adding wget and bzip2
RUN apt-get install -y wget bzip2

# Add sudo
RUN apt-get -y install sudo

# ubuntu packages
Run apt-get install -y git libsdl2-dev libzmq3-dev binutils-dev libdw-dev libgflags-dev libnuma-dev cmake libgoogle-glog-dev sudo

# Add user ubuntu with no password, add to sudo group
RUN adduser --disabled-password --gecos '' ubuntu
RUN adduser ubuntu sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER ubuntu
WORKDIR /home/ubuntu/
RUN chmod a+rwx /home/ubuntu/
#RUN echo `pwd`

# Anaconda installing
RUN wget https://repo.continuum.io/archive/Anaconda3-2020.02-Linux-x86_64.sh
RUN bash Anaconda3-2020.02-Linux-x86_64.sh -b
RUN rm Anaconda3-2020.02-Linux-x86_64.sh

# Set path to conda
#ENV PATH /root/anaconda3/bin:$PATH
ENV PATH /home/ubuntu/anaconda3/bin:$PATH

# Updating Anaconda packages
RUN conda update conda
RUN conda update anaconda
RUN conda update --all



#test Conda
COPY conda.txt .
CMD cat conda.txt


#install conda packages

RUN conda install numpy pyyaml mkl=2019.1 mkl-include=2019.1 setuptools cmake cffi typing
RUN conda install -c mingfeima mkldnn


RUN echo "star conda is ready:"
#building Torch
RUN export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
RUN git clone https://github.com/TorchCraft/TorchCraftAI --recursive
RUN cd TorchCraftAI
RUN cd ./3rdparty/pytorch/tools/REL_WITH_DEB_INFO=1 && python build_libtorch.py

#building zstd 
RUN curl -sSL https://github.com/facebook/zstd/archive/v1.3.3.tar.gz | tar xvzf -
RUN cd ./zstd-1.3.3/build/cmake
RUN cmake . -DCMAKE_C_FLAGS=-fPIC -DCMAKE_CXX_FLAGS=-fPIC -DZSTD_BUILD_STATIC=ON -DCMAKE_BUILD_TYPE=Release -DZSTD_LEGACY_SUPPORT=0
RUN make -j4
RUN sudo make install
RUN cd ../../../


RUN cd 3rdparty/openbw
RUN mkdir -p build
RUN cd build
RUN cmake .. -DCMAKE_BUILD_TYPE=release -DOPENBW_ENABLE_UI=1
RUN make -j4
RUN sudo make install



