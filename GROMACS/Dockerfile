ARG BASE_IMAGE_CUDA_VERSION=12.8.1
ARG BASE_IMAGE_UBUNTU_VERSION=24.04

FROM nvidia/cuda:${BASE_IMAGE_CUDA_VERSION}-devel-ubuntu${BASE_IMAGE_UBUNTU_VERSION} AS base

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install --no-install-recommends --yes \
    build-essential \
    cmake && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


ARG GROMACS_VERSION="gromacs-2025.1"
ARG CPU_CORES=12

ADD https://ftp.gromacs.org/gromacs/$GROMACS_VERSION.tar.gz .

RUN tar xfz $GROMACS_VERSION.tar.gz && \
    cd $GROMACS_VERSION && \
    mkdir build

RUN cd $GROMACS_VERSION/build && \
    cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=CUDA

RUN cd $GROMACS_VERSION/build && \
    make -j$CPU_CORES

RUN cd $GROMACS_VERSION/build && \
    make check -j$CPU_CORES

RUN cd $GROMACS_VERSION/build && \
    make install 

ENV PATH=/usr/local/gromacs/bin:$PATH
