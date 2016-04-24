#This comes from 
#https://github.com/NVIDIA/nvidia-docker/blob/master/ubuntu-14.04/cuda/7.5/runtime/Dockerfile
#https://github.com/NVIDIA/nvidia-docker/blob/master/ubuntu-14.04/cuda/7.5/runtime/cudnn4/Dockerfile
#Since nvidia's repository doesn't serve sha1 hash key for cuda, I am using xenial multiverse repository


FROM ubuntu:xenial
MAINTAINER laket <laket72@gmail.com>

LABEL com.nvidia.volumes.needed="nvidia_driver"

ENV NVIDIA_GPGKEY_SUM bd841d59a27a406e513db7d405550894188a4c1cd96bf8aa4f82f1b39e0b5c1c
ENV NVIDIA_GPGKEY_FPR 889bee522da690103c4b085ed88c3d385c37d3be

RUN apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/GPGKEY && \
    apt-key adv --export --no-emit-version -a $NVIDIA_GPGKEY_FPR | tail -n +5 > cudasign.pub && \
    echo "$NVIDIA_GPGKEY_SUM  cudasign.pub" | sha256sum -c --strict - && rm cudasign.pub 

RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1404/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list
Run echo "deb http://jp.archive.ubuntu.com/ubuntu/ xenial multiverse" >> /etc/apt/sources.list.d/cuda.list
Run echo "deb http://jp.archive.ubuntu.com/ubuntu/ xenial-updates multiverse" >> /etc/apt/sources.list.d/cuda.list

ENV CUDA_VERSION 7.5
LABEL com.nvidia.cuda.version="7.5"
ENV CUDNN_VERSION 4
LABEL com.nvidia.cudnn.version="4"

#http://askubuntu.com/questions/760796/how-to-fix-apt-signature-by-key-uses-weak-digest-algorithm-sha1-after-install
#https://wiki.debian.org/RepositoryFormat

RUN apt-get update && apt-cache search nvidia-cuda

RUN apt-get update && apt-get install -y --no-install-recommends --force-yes \
        nvidia-cuda-toolkit \
        libcudnn4=4.0.7 && \
    rm -rf /var/lib/apt/lists/*

ENV PATH /usr/lib/nvidia-cuda-toolkit/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/lib/x86_64-linux-gnu/:${LD_LIBRARY_PATH}
