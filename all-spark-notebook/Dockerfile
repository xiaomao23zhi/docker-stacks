# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG OWNER=jupyter
ARG BASE_CONTAINER=registry.cn-hangzhou.aliyuncs.com/gcrioo/pyspark-notebook:spark-3.1.1
FROM $BASE_CONTAINER

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

USER root

# RSpark config
ENV R_LIBS_USER "${SPARK_HOME}/R/lib"
RUN fix-permissions "${R_LIBS_USER}"

# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# R packages including IRKernel which gets installed globally.
RUN mamba install --quiet --yes \
    'binutils_impl_linux-64=2.36.1' \
    'binutils_linux-64=2.36' \
    'r-base=4.1.1' \
    'r-ggplot2' \
    'r-irkernel' \
    'r-rcurl' \
    'r-sparklyr' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Spylon-kernel
RUN mamba install --quiet --yes 'spylon-kernel' && \
    mamba clean --all -f -y && \
    python -m spylon_kernel install --sys-prefix && \
    rm -rf "/home/${NB_USER}/.local" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
