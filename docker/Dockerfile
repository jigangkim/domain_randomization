FROM nvidia/cuda:10.2-cudnn7-runtime-ubuntu16.04

RUN apt-get update && apt-get -y upgrade
RUN  apt-get update &&  apt-get install -y software-properties-common
RUN cat /etc/apt/sources.list
# RUN  add-apt-repository ppa:jonathonf/python-3.6
# RUN  add-apt-repository ppa:libccd-debs/ppa
# RUN  add-apt-repository ppa:fcl-debs/ppa
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-add-repository ppa:dartsim/ppa
RUN  apt-get update &&   apt-get install -y \
      python3.6 git curl \
      build-essential cmake \
      libeigen3-dev \
      libccd-dev \
      libfcl-dev \
      libassimp-dev \
      libboost-regex-dev \
      libopenscenegraph-dev \
      libqt4-dev libqt4-opengl-dev \
      coinor-libipopt-dev libxi-dev libxmu-dev freeglut3-dev libflann-dev libnlopt-dev \
      gawk \
      python3-dev \
      python3.4-dev \
      libtinyxml2-dev \
      liburdfdom-dev \
      doxygen \
      python3.6-dev unzip \
      libxrandr-dev libosmesa-dev libxinerama-dev libxcursor-dev \
      libtcmalloc-minimal4 \
      swig

ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/mesa/libGL.so:/usr/lib/libtcmalloc_minimal.so.4
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/root/.mujoco/mjpro200/bin


# Compiling libraries

RUN mkdir /root/lib
WORKDIR /root/lib
RUN curl -L https://github.com/Kitware/CMake/releases/download/v3.14.0-rc3/cmake-3.14.0-rc3.tar.gz > cmake-3.14.0-rc3.tar.gz
RUN tar xzf cmake-3.14.0-rc3.tar.gz
RUN cd cmake-3.14.0-rc3; ./bootstrap;  make install
RUN git clone git://github.com/OctoMap/octomap.git
RUN cd octomap; git checkout tags/v1.8.1 -b v1.8.1; mkdir build; cd build; cmake ..; make -j$(nproc);  make install
RUN curl -L https://bitbucket.org/odedevs/ode/downloads/ode-0.16.tar.gz > ode-0.16.tar.gz
RUN tar xzf ode-0.16.tar.gz
RUN cd ode-0.16; cd build; cmake ..; make -j$(nproc);  make install
RUN git clone https://github.com/bulletphysics/bullet3.git
RUN cd bullet3; git checkout tags/2.88 -b 2.88; ./build_cmake_pybullet_double.sh; cd build_cmake;  make install
RUN curl -L https://www.roboti.us/download/mujoco200_linux.zip > /root/lib/mujoco200_linux.zip
RUN mkdir /root/.mujoco; unzip /root/lib/mujoco200_linux.zip -d /root/.mujoco/mujoco200
RUN touch /root/.mujoco/mjkey.txt
RUN curl -L http://nixos.org/releases/patchelf/patchelf-0.9/patchelf-0.9.tar.bz2 > patchelf-0.9.tar.bz2
RUN tar xf patchelf-0.9.tar.bz2
RUN cd patchelf-0.9; ./configure; make -j$(nproc);  make install
RUN git clone https://github.com/glfw/glfw; cd glfw; mkdir build; cd build; cmake ..; make -j$(nproc);  make install
RUN git clone https://github.com/dartsim/dart
RUN cd dart; git checkout tags/v6.6.0 -b v6.6.0; mkdir build; cd build; cmake ..; make -j$(nproc);  make install
RUN  ldconfig /usr/local/lib
RUN rm -rf /root/lib

# Installing pip and pipenv
RUN curl https://bootstrap.pypa.io/get-pip.py | python3.6
RUN  pip3 install pipenv

# Cloning domain randomization
RUN mkdir /root/work
WORKDIR /root/work
RUN mkdir -p -m 0600 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts

RUN git clone https://github.com/sharadmv/domain-randomization.git
WORKDIR /root/work/domain-randomization
RUN git pull origin master
ENV PIPENV_TIMEOUT=9999
# ENV PIP_NO_CACHE_DIR=false
RUN pipenv install tensorflow
RUN mkdir /root/.aws; echo '[default]\nregion = us-west-2' > ~/.aws/config

WORKDIR /root/work/
RUN git clone https://github.com/quanvuong/baselines.git

WORKDIR /root/work/domain-randomization
ENV PIPENV_TIMEOUT=9999
# ENV PIP_NO_CACHE_DIR=false
RUN pipenv install -e ../baselines

ENV PIPENV_TIMEOUT=9999
# ENV PIP_NO_CACHE_DIR=false
RUN pipenv run pip install https://download.pytorch.org/whl/cpu/torch-1.0.1.post2-cp36-cp36m-linux_x86_64.whl