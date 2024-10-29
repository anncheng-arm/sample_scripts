#### Build the Docker image that includes all necessary dependencies for running ML inference:
`docker build -t mlperf_image .`

#### Start a Docker container from the image and run the ML inference script for TensorFlow, ResNet50, on the CPU:
`docker run -it mlperf_image /bin/bash -c "./run_local.sh tf resnet50 cpu; exec bash" `
