ARG BASE_IMAGE_CUDA_VERSION=12.8.1
ARG BASE_IMAGE_UBUNTU_VERSION=24.04

FROM nvidia/cuda:${BASE_IMAGE_CUDA_VERSION}-runtime-ubuntu${BASE_IMAGE_UBUNTU_VERSION} AS final


RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install --no-install-recommends --yes \
    wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ARG MINIFORGE_NAME=Miniforge3
ARG MINIFORGE_VERSION=24.9.2-0

ENV CONDA_DIR=/opt/conda
ENV PATH=${CONDA_DIR}/bin:${PATH}

# Miniforge install with cleanup
RUN wget --no-hsts --quiet https://github.com/conda-forge/miniforge/releases/download/${MINIFORGE_VERSION}/${MINIFORGE_NAME}-${MINIFORGE_VERSION}-Linux-$(uname -m).sh -O /tmp/miniforge.sh && \
    /bin/bash /tmp/miniforge.sh -b -p ${CONDA_DIR} && \
    rm /tmp/miniforge.sh && \
    conda install -y openmm cuda-version=12 && \
    conda clean --tarballs --index-cache --packages --yes && \
    find ${CONDA_DIR} -follow -type f -name '*.a' -delete && \
    find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete && \
    conda clean --force-pkgs-dirs --all --yes 