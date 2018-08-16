FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

MAINTAINER Han Liang Tan

# Redundant, but increases clarity
USER root

# don't run as root within the container
RUN groupadd --gid 1000 thldockergroup \
    && adduser --disabled-password --uid 1000 thldocker
    && usermod -a -G thldockergroup thldocker

RUN apt-get update && apt-get install --yes \
    build-essential \
    curl \
    fish \
    git \
    sudo \
    vim

RUN chown thldocker:thldockergroup /home/thldocker/ \
    && chmod 754 /home/thldocker/ \
    && chsh --shell /usr/bin/fish

USER thldocker

RUN mkdir /home/thldocker/code
WORKDIR /home/thldocker/code

RUN curl --show-error --output /home/thldocker/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && chmod +x /home/thldocker/miniconda.sh \
    && /home/thldocker/miniconda.sh -p /home/thldocker/miniconda -b \
    && rm /home/thldocker/miniconda.sh

# Allow fish shell to run "conda activate", if additional environments are required
# https://stackoverflow.com/questions/42871313/cannot-run-source-activate-with-conda-in-fish-shell
ENV fish_config="/home/thldocker/.config/fish/config.fish"

RUN mkdir --parents /home/thldocker/.config/fish/ \
    && touch $fish_config \
    && echo "source /home/thldocker/miniconda/etc/fish/conf.d/conda.fish" > $fish_config

ENV base_conda=/home/thldocker/miniconda/bin/conda

RUN $base_conda install conda-build \
    && $base_conda config --set auto_update_conda False

ENV PATH /home/thldocker/miniconda/bin:$PATH

RUN conda install --yes \
    python=3

RUN conda install --channel pytorch --yes \
    cuda90 \
    pytorch \
    torchvision

# This will unfortunately change python3 to Intel's python distribution
RUN conda install --channel intel --no-update-deps --yes \
    numpy \
    scikit-learn \
    scipy 

RUN conda install --channel conda-forge --yes \
    matplotlib \
    jupyterlab

RUN conda install --channel anaconda --yes \
    pandas

EXPOSE 8888

ENTRYPOINT ["/usr/bin/fish"]
