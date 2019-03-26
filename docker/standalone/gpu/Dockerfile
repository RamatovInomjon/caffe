FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04
MAINTAINER simcam

RUN apt-get update && apt-get upgrade -y && apt-get dist-upgrade -y && apt-get autoremove \
        && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        qt5-default \
        libvtk6-dev \
        zlib1g-dev \
        libjpeg-dev \
        libwebp-dev \
        libpng-dev \
        libtiff5-dev \
        libjasper-dev  \
        libopenexr-dev \
        libgdal-dev \
        libdc1394-22-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libvorbis-dev \
        libtheora-dev \
        libxvidcore-dev \
        libx264-dev \
        yasm \
        libopencore-amrnb-dev \
        libopencore-amrwb-dev \
        libv4l-dev \
        libxine2-dev \
        libtbb-dev \
        libeigen3-dev \
        python-tk \
        python3-dev \
        python3-tk \
        python3-numpy \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-scipy && \
    rm -rf /var/lib/apt/lists/*


ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT

# FIXME: clone a specific git tag and use ARG instead of ENV once DockerHub supports this.


RUN git clone -b ssd --depth 1 https://github.com/RamatovInomjon/caffe.git . && \
    for req in $(cat python/requirements.txt) pydot; do pip install $req; done && \
    mkdir build && \
    cp Makefile.config.example Makefile.config && \
    cd opencv \
    && mkdir build \
    && cd build \
    && cmake ../ \
    && make install  \
    && rm -r /opencv
RUN ln -s \
  /usr/local/python/cv2/python-3.7/cv2.cpython-37m-x86_64-linux-gnu.so \
/usr/local/lib/python3.7/site-packages/cv2.so
RUN make all -j"$(nproc)" && \ 
    make test -j"$(nproc)" && \ 
    make runtest -j"$(nproc)" && \ 
    make pycaffe -j"$(nproc)" \

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR /
