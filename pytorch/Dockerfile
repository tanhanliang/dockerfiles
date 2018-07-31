FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

MAINTAINER Han Liang Tan 

RUN chmod 777 /home

ENV HOME=/home

RUN apt-get update && apt-get install --yes \
    curl \
    git \
    sudo \
    build-essential \
    fish

RUN chsh -s /usr/bin/fish

# don't run as root within the container
RUN groupadd --gid 1000 thldockergroup && \
    adduser --uid 999 thldocker

USER thldocker 

RUN curl --show-error --output ~/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && chmod +x ~/miniconda.sh \
    && ~/miniconda.sh -p /home/miniconda -b \
    && rm ~/miniconda.sh

ENV PATH=/home/miniconda/bin:$PATH
 
RUN conda install conda-build \
    && conda create --yes --name pytorch python=3.6

ENV CONDA_DEFAULT_ENV=pytorch
ENV CONDA_PREFIX=/home/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH

RUN conda activate pytorch

RUN conda config --set auto_update_conda False

RUN conda install --channel pytorch --yes \
    pytorch \
    torchvision \
    cuda90

# accelerated with Intel Math Kernel Library
RUN conda install --channel intel --yes \
    numpy \
    scipy \
    scikit-learn

RUN conda install --channel conda-forge --yes \
    matplotlib \
    jupyterlab

EXPOSE 8888

ENTRYPOINT ["/usr/bin/fish"]
