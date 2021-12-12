# Dockerfiles

## Requirements
### Docker
In order to use one of those images oyu must have Docker installed. If you need to install Docker, you can follow the tutorial available on the [Docker website](https://docs.docker.com/engine/install/).

### CUDA
If you want to build a docker image which uses NVIDIA graphic cards, you must have a CUDA-compatible GPU and install the ```nvidia-docker``` image.

Firstly, ensure that you install the appropriate NVIDIA drivers. To install them, you can do it downloading the newest driver version through the [official NVIDIA website](https://developer.nvidia.com/cuda-downloads). You can run the command ```nvidia-smi``` to check the information about the NVIDIA GPU and, eventually the CUDA version. On Ubuntu, you'll get something like that

        +-----------------------------------------------------------------------------+
        | NVIDIA-SMI 470.86       Driver Version: 470.86       CUDA Version: 11.4     |
        |-------------------------------+----------------------+----------------------+
        | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
        | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
        |                               |                      |               MIG M. |
        |===============================+======================+======================|
        |   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 N/A |                  N/A |
        | 10%   39C    P0    N/A /  N/A |    420MiB /  1996MiB |     N/A      Default |
        |                               |                      |                  N/A |
        +-------------------------------+----------------------+----------------------+

        +-----------------------------------------------------------------------------+
        | Processes:                                                                  |
        |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
        |        ID   ID                                                   Usage      |
        |=============================================================================|
        |  No running processes found                                                 |
        +-----------------------------------------------------------------------------+

In that case, I'm using a GPU with CUDA 11.4.
You will also need to install the [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker), or ```nvidia-docker``` to enable GPU device access within Docker containers.

## Usage
### Building a Dockerfile
There are two ways to build a Dockerfile: first, downloading a prebuilt image from [Docker Hub](https://hub.docker.com/) and second, creating your own Dockerfile.

If you download a prebuilt image, you basically run the following command:

```bash
$ docker pull <user-docker>/<image-name>:<version>
```

If you create your own Dockerfile, you have to build the image from scratch, going to its directory and running the following command:


```bash
$ docker build -f <Dockerfile-name> -t <name-image> .
```

### Running PyTorch images
If you built your Dockerfile or downloaded from Docker Hub, you can run your image inside a Docker container. You can also use your current directory as a work directory using the ```--volume``` flag and setting the permissions correctly. To run a Docker image, you can run the following command:

```sh
docker run -it  \
  --name=<container-name>
  --gpus=all \
  --ipc=host \
  --user="$(id -u):$(id -g)" \
  --volume="$PWD:/app" \
  <name-image> bash
```

### Starting a Docker container
You can start a created Docker container running the following command:

```bash
$ docker start <container-name>
```

And executing running

```bash
$ docker exec -it <container-name> bash
```



