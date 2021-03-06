# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM jupyter/pyspark-notebook

MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# RSpark config
ENV R_LIBS_USER $SPARK_HOME/R/lib

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_USER

# R packages
RUN conda config --add channels r && \
    conda install --quiet --yes \
    'r-base=3.3*' \
    'r-irkernel=0.6*' \
    'r-ggplot2=2.1*' \
    'r-rcurl=1.95*' && conda clean -tipsy

# Apache Toree kernel
RUN pip --no-cache-dir install toree==0.1.0.dev7
RUN jupyter toree install --user

USER root
RUN apt-get update
RUN apt-get upgrade -y -o Dpkg::Options::="--force-confdef" -o DPkg::Options::="--force-confold" 
RUN apt-get install -y man-db libc6 libc6-dev build-essential wget curl file vim screen tmux unzip
RUN apt-get install -y pkg-config cmake gfortran gettext libreadline-dev libncurses-dev libpcre3-dev 
RUN apt-get install -y libgnutls28-dev python python-yaml python-m2crypto python-crypto msgpack-python
RUN apt-get install -y python-dev python-setuptools supervisor python-zmq python-jinja2
RUN apt-get install -y python-requests python-isodate python-git python-pip 
RUN apt-get clean 
RUN pip install --upgrade PyDrive google-api-python-client jsonpointer jsonschema tornado sphinx pygments nose readline mistune invoke 
RUN git clone --recursive --branch 3.x https://github.com/ipython/ipython.git; cd ipython; python setup.py install; cd ..; rm -rf ipython

RUN mkdir -p /opt/julia_0.5.0 && curl -s -L https://status.julialang.org/download/linux-x86_64 | tar -C /opt/julia_0.5.0 -x -z --strip-components=1 -f - 
RUN ln -fs /opt/julia_0.5.0 /opt/julia 
RUN echo "PATH=\"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/opt/julia/bin\"" > /etc/environment && echo "export PATH" >> /etc/environment && echo "source /etc/environment" >> /root/.bashrc 
RUN /opt/julia/bin/julia -e 'Pkg.add("IJulia")'
