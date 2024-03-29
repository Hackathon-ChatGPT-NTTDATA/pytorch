# 0. Image and labels
FROM ubuntu:latest
LABEL "guru.raraavis.creator"="blog@raraavis.guru"
LABEL "guru.raraavis.version"="1.1.0"
LABEL "guru.raraavis.release-date"="31/10/2021"
LABEL "guru.raraavis.description"="Raspberry Pi image with JupyterLab"

# 1. Environment vars
ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_OPTIONS=--max-old-space-size=768
ARG TZ=Europe/Madrid
ENV NODE_VERSION=16
ENV JUPYTER_ID=1000
ENV TZ=$TZ

# 2. Install packages
# 2.1 Update system and install necessary packages
RUN     apt-get update && \
        apt-get install -y --no-install-recommends \
        python3-dev \
        python3-venv \
        libatlas-base-dev \
        libblas-dev \
        liblapack-dev \
        gfortran \
        tzdata \
        software-properties-common \
        sudo \
        gosu \
        bzip2 \
        git \
        dumb-init \
        curl && \
        curl -sL https://deb.nodesource.com/setup_$NODE_VERSION.x | bash - && \
        apt-get install -y --no-install-recommends nodejs && \
        rm -rf /var/lib/apt/lists/* && \
        rm -rf /tmp/*
# 2.2 Install pip
ADD     https://bootstrap.pypa.io/get-pip.py get-pip.py
RUN     python3 get-pip.py
RUN     python3 -m pip config --global set global.extra-index-url https://www.piwheels.org/simple
# 2.3 Install pip packages and Jupyter
RUN     python3.9 -m pip install --upgrade \
        conda \
        virtualenv \
        ipykernel \
        jupyter \
        jupyterlab \
        jupyterlab-git \
        jupyter_contrib_nbextensions \
        autopep8

RUN     jupyter lab build
RUN     jupyter contrib nbextension install --system

# 3. Add jupyter user
RUN     adduser --uid $JUPYTER_ID --disabled-password --gecos '' jupyter
RUN     adduser jupyter sudo
RUN     echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# 4. Install Miniconda
WORKDIR /home/jupyter
ADD --chown=jupyter     http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh Miniconda3.sh
RUN     chmod 755 Miniconda3.sh
RUN     md5sum Miniconda3.sh
RUN     ./Miniconda3.sh -b -p /home/jupyter/miniconda3
RUN     ./miniconda3/bin/conda config --add channels rpi
RUN     rm ./Miniconda3.sh

# 5. Configure container
RUN     mkdir -p /home/jupyter/notebooks
RUN     chown -R jupyter:jupyter /home/jupyter
ONBUILD SHELL   ["/bin/bash","-c"]
VOLUME /home/jupyter/notebooks
EXPOSE 8888
USER jupyter

# 6. Execute jupyter
CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--notebook-dir=/mnt/jupyter/notebooks", "--ServerApp.token=", "--ServerApp.password="]