FROM nvcr.io/nvidia/tensorrt:20.03-py3

### update apt and install libs
RUN apt-get update &&\
    apt-get install -y vim cmake libsm6 libxext6 libxrender-dev libgl1-mesa-glx git &&\
    rm -rf /var/lib/apt/lists/*

### create folder
RUN mkdir ~/space &&\
    mkdir /root/.pip

### set pip source
# COPY ./pip.conf /root/.pip

### python
RUN pip3 install --no-cache-dir --upgrade pip

### pytorch
RUN pip3 install --no-cache-dir torch==1.6.0 torchvision==0.7.0

### install mmcv
RUN pip3 install --no-cache-dir https://openmmlab.oss-accelerate.aliyuncs.com/mmcv/dist/1.1.5/torch1.6.0/cu102/mmcv_full-1.1.5%2Btorch1.6.0%2Bcu102-cp36-cp36m-manylinux1_x86_64.whl

### git mmdetection
RUN git clone --depth=1 https://github.com/open-mmlab/mmdetection.git /root/space/mmdetection

### install mmdetection
RUN cd /root/space/mmdetection &&\ 
    pip3 install --no-cache-dir -r requirements.txt &&\
    python3 setup.py develop

### git amirstan plugin
RUN git clone --depth=1 https://github.com/grimoire/amirstan_plugin.git /root/space/amirstan_plugin &&\ 
    cd /root/space/amirstan_plugin &&\ 
    git submodule update --init --progress --depth=1

### install amirstan plugin
RUN cd /root/space/amirstan_plugin &&\ 
    mkdir build &&\
    cd build &&\
    cmake .. &&\
    make -j10

ENV AMIRSTAN_LIBRARY_PATH=/root/space/amirstan_plugin/build/lib

### git torch2trt_dynamic
RUN git clone --depth=1 https://github.com/grimoire/torch2trt_dynamic.git /root/space/torch2trt_dynamic

### install torch2trt_dynamic
RUN cd /root/space/torch2trt_dynamic &&\
    python3 setup.py develop

### git mmdetection-to-tensorrt
RUN git clone --depth=1 https://github.com/grimoire/mmdetection-to-tensorrt.git /root/space/mmdetection-to-tensorrt

### install mmdetection-to-tensorrt
RUN cd /root/space/mmdetection-to-tensorrt &&\
    python3 setup.py develop

WORKDIR /root/space

CMD [ "--help" ]
ENTRYPOINT [ "mmdet2trt" ]
