# docker_for_leela-zero
This repository provides a dockerfile for building a runtime environment for [leela-zero](https://github.com/leela-zero/leela-zero).

Leela-zero is :
>A Go program with no human provided knowledge. Using MCTS (but without Monte Carlo playouts) and a deep residual convolutional neural network stack.  
This is a fairly faithful reimplementation of the system described in the Alpha Go Zero paper "Mastering the Game of Go without Human Knowledge". For all intents and purposes, it is an open source AlphaGo Zero.

# 1. Prerequisites  
The following are additions or limitations to the requirements of leela-zero. 
+ Linux x86_64
+ NVIDIA GPU
+ Docker or Docker-CE
+ nvidia-docker >= 2.0

# 2. Installation
## 2.1 Download
Clone this repository:  
```console
$ git clone https://github.com/GrahamML/docker_for_leela-zero.git
```
## 2.2 Build the docker image


```console
$ cd ./docker_for_leela-zero/dockerfile
$ docker build --tag=[image_name:tag] .
```  
+ This dockerfile install the requirement packages on the [NVIDIA OpenCL official docker image](https://hub.docker.com/r/nvidia/opencl), and make the leela-zero runtime.
+ The following shows the version of Ubuntu, CUDA, and TensorRT in this container. 

    | Ubuntu | OpenCL              |
    |--------|---------------------|
    | 16.04  | OpenCL 1.2 CUDA     |

# 3. How to run
## 3.1 Start the docker image
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
## 3.2 Launch the leela-zero
Launch the leela-zero in this container. The following is an example of launching in test mode and AutoGTP (self play) mode.
### 3.2.1. Test mode
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
### 3.2.2. AutoGTP mode
```console
$ cd leela-zero/build/autogtp
$ ./autogtp
AutoGTP v18
Using 1 game thread(s) per device.
Starting tuning process, please wait...
:
Starting GTP commands sent.
1 (B Q16) 2 (W Q3) 3 (B C16) 4 (W H11) 5 (B D4) 6 (W Q6) 7 (B E16) 8 (W R14) 9 (B C6) 10 (W O17) 11 (B O16) 12 (W N16) 13 (B O15) 14 (W Q17) 15 (B R17) 16 (W P17) 17 (B R16) 18 (W R12) 19 (B O13) 20 (W M17) 21 (B L16) 22 (W N15) 23 (B Q13) 24 (W N13) 25 (B N12) 26 (W M13) 27 (B Q9) 28 (W R10) 29 (B Q12) 30 (W R13) 31 (B R9) 32 (W Q10) ...
:
412 (W K6) 413 (B pass) 414 (W pass) Game has ended.
Score: B+31.5
Winner: black
```  
+ Refer to the [leela-zero's readme](https://github.com/leela-zero/leela-zero/blob/master/README.md) for more information on onptions and launch modes.
# 4. License  
[MIT](https://github.com/GrahamML/docker_for_leela-zero/blob/master/LICENSE)
