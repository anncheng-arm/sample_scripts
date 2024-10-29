FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y build-essential python3-pip git wget && \
    rm -rf /var/lib/apt/lists/*
RUN pip3 install pybind11

# Clone the mlcommons/inference repository and enter the loadgen directory
RUN git clone --depth 1 --recurse-submodules https://github.com/mlcommons/inference.git mlperf_inference

# Compile and install loadgen
WORKDIR /mlperf_inference/loadgen
RUN CFLAGS="-std=c++14" python3 setup.py develop --user

# Set the working directory
WORKDIR /mlperf_inference/vision/classification_and_detection

# Replace mlperf_loadgen with mlcommons_loadgen in setup.py to workaround module rename problem
RUN sed -i 's/mlperf_loadgen/mlcommons_loadgen/g' setup.py && \
    python3 setup.py develop

# Install TensorFlow and TensorFlow-IO
RUN pip install tensorflow tensorflow-io

# Set environment variables
ENV TF_ENABLE_ONEDNN_OPTS=1
ENV ONEDNN_DEFAULT_FPMATH_MODE=BF16

# Download the model file and create a fake ImageNet dataset
RUN wget -q https://zenodo.org/record/2535873/files/resnet50_v1.pb && \
    ./tools/make_fake_imagenet.sh

# Set MODEL_DIR and DATA_DIR environment variables
ENV MODEL_DIR=/mlperf_inference/vision/classification_and_detection
ENV DATA_DIR=/mlperf_inference/vision/classification_and_detection/fake_imagenet
