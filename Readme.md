# Dockerfile for GPU enabled rstudio server with keras+tensorflow

Coaxing Rstudio+Keras+tensorflow to use your local GPU can be tricky. This is one solution to that problem.

This repo contains a plausible (tested on NVIDIA GeForce 755M) docker container with rstudio server and the tiddyverse and keras+tensorflow built on top of `nvidia/cuda:9.0-cudnn7-runtime`.

The objective is to be able to run the code from the [Deep learnijng for R](https://github.com/jjallaire/deep-learning-with-r-notebooks) book given an NVIDIA GPU with drivers installed. The Docker container will install the CUDA and cudnn libraries needed.

There is also an `.Rmd` file with some code snippets from the book, so that you can test if the setup works.

See [this](https://github.com/rocker-org/rocker/issues/273),  [this](https://hub.docker.com/r/rocker/ml/) and [this](https://github.com/rstudio/keras/issues/207) for other approaches and discussions.

## Pre-requisites

1. **NVIDIA gpu**. If you don't need to run on the GPU, you are better off just running code in your local RStudio or the excellent [verse container](https://hub.docker.com/r/rocker/verse/) from the rocker project. If you are using linux and are unsure if your NVIDIA gpu is being used, see [this](https://unix.stackexchange.com/questions/16407/how-to-check-which-gpu-is-active-in-linux)

2. **docker**. If you have not updated your docker installation in a while, or don't have one, you can get the latest version of `docker-ce` for your OS from [here](https://docs.docker.com/install/).

3. **nvidia-docker**. This is needed to be able to run docker containers with the nvidia CUDA cudnn backends we need. Follow the installation instructions [here](https://github.com/NVIDIA/nvidia-docker)


## Getting started

1. Clone this repo and from the command line, switch to the repo directory.

2. From the repo directory run the following (and then get a coffee, typically, the build process will take some time) :
```
sudo nvidia-docker build --rm -t gpu-keras-tidyverse:1.0 gpu-keras-tidyverse
```
3.
