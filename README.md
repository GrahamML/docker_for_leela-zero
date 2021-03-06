# docker-for-leela-zero
This repository provides a dockerfile for building a runtime environment for [leela-zero](https://github.com/leela-zero/leela-zero).  

Leela-zero is :
>A Go program with no human provided knowledge. Using MCTS (but without Monte Carlo playouts) and a deep residual convolutional neural network stack.  
This is a fairly faithful reimplementation of the system described in the Alpha Go Zero paper "Mastering the Game of Go without Human Knowledge". For all intents and purposes, it is an open source AlphaGo Zero.  

_日本語の説明は[こちら](https://github.com/GrahamML/docker-for-leela-zero/blob/master/README_JP.md)を参照ください。_

# 1. Prerequisites  
The following are additions or limitations to the requirements of leela-zero. 
+ Linux x86_64
+ NVIDIA GPU
+ Docker
+ nvidia-docker2 (Docker >= 1.12) or nvidia-container-toolkit (Docker >= 19.03)  

_Learn more about how to install [docker](https://github.com/Microsoft/MMdnn/blob/master/docs/InstallDockerCE.md) and [nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#quickstart)_

# 2. Installation
## 2.1. Download
Clone this repository:  
```console
$ git clone https://github.com/GrahamML/docker-for-leela-zero.git
```
## 2.2. Build the docker image
```console
$ cd ./docker-for-leela-zero/dockerfile
$ docker build --tag=[image_name:tag] .
```  
+ This dockerfile install the requirement packages on the [NVIDIA OpenCL official docker image](https://hub.docker.com/r/nvidia/opencl), and make the leela-zero runtime.
+ The following is an example of a build command.  
    ```
    $ docker build --tag=leela-zero:latest . 
    ```
+ The following shows the version of Ubuntu and OpenCL in this container. 

    | Ubuntu | OpenCL              |
    |--------|---------------------|
    | 16.04  | OpenCL 1.2 CUDA     |

# 3. How to run
## 3.1. Start the docker image
Start the docker image as follows:  
```console
$ docker run \
    -it \
    --net host \
    --runtime nvidia \
    --entrypoint /bin/bash \
    --name [container_name] \
    [image_name:tag]
```  
+ The example above is for `nvidia-docker2` case. If it's `nvidia-container-toolkit` case, change it appropriately.  

## 3.2. Launch the leela-zero
Launch the leela-zero in this container. The following is an example of launching in test mode and AutoGTP (self play) mode.
### Test mode
```console
$ cd leela-zero/build
$ ./tests
Running main() from gtest_main.cc
[==========] Running 13 tests from 2 test cases.
[----------] Global test environment set-up.
BLAS Core: built-in Eigen 3.3.7 library.
Detecting residual layers...v1...8 channels...1 blocks.
Initializing OpenCL (autodetecting precision).
Detected 1 OpenCL platforms.
Platform version: OpenCL 1.2 CUDA 10.1.152
Platform profile: FULL_PROFILE
Platform name:    NVIDIA CUDA
Platform vendor:  NVIDIA Corporation
Device ID:     0
:
[----------] Global test environment tear-down
[==========] 13 tests from 2 test cases ran. (145254 ms total)
[  PASSED  ] 13 tests.
```
### AutoGTP mode
```console
$ cd leela-zero/build/autogtp
$ ./autogtp
AutoGTP v18
Using 1 game thread(s) per device.
Starting tuning process, please wait...
:
Starting GTP commands sent.
1 (B Q16) 2 (W Q3) 3 (B C16) 4 (W H11) 5 (B D4) 6 (W Q6) 7 (B E16) 8 (W R14) 9 (B C6) 10 (W O17)...
:
412 (W K6) 413 (B pass) 414 (W pass) Game has ended.
Score: B+31.5
Winner: black
```  
+ Refer to the [leela-zero's readme](https://github.com/leela-zero/leela-zero/blob/master/README.md) for more information on onptions and launch modes.

## 3-3. Network Update  
The network parameters can be updated in a container as follows:
```console
$ cd leela-zero/build
$ curl -O https://zero.sjeng.org/best-network
```

# 4. Communitacion with Lizzie  
See this [wiki](https://github.com/GrahamML/docker_for_AQ/wiki/Communitacion-with-Lizzie).  

![Select](https://github.com/GrahamML/docker_for_AQ/wiki/images/Communitacion-with-Lizzie/Fig9.png)

# 5. License  
[MIT](https://github.com/GrahamML/docker_for_leela-zero/blob/master/LICENSE)
