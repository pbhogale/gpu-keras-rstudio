# Dockerfile for GPU enabled rstudio server with keras+tensorflow

Coaxing Rstudio+Keras+tensorflow to use your local GPU can be tricky. This is one solution to that problem.

**Disclaimer :** The Dockerfile needs re-factoring to be more efficient during the build process.
**Inspired by** https://github.com/rocker-org/ml/.

This repo contains a plausible (tested on NVIDIA GeForce 755M) docker container with rstudio server and the tidyverse and keras+tensorflow built on top of `nvidia/cuda:9.0-cudnn7-runtime`. The objective is to be able to run the code from the [Deep learning in R](https://github.com/jjallaire/deep-learning-with-r-notebooks) by [F. Chollet](https://twitter.com/fchollet). book given an NVIDIA GPU with drivers installed. The Docker container will install the CUDA and cudnn libraries needed.

There is also an `.Rmd` file with some code snippets from the book, so that you can test if the setup works.

See [this](https://github.com/rocker-org/rocker/issues/273),  [this](https://hub.docker.com/r/rocker/ml/) and [this](https://github.com/rstudio/keras/issues/207) for other approaches and discussions.

## Pre-requisites

1. **NVIDIA gpu**. If you don't need to run on the GPU, you are better off just running code in your local RStudio or the excellent [verse container](https://hub.docker.com/r/rocker/verse/) from the rocker project. If you are using linux and are unsure if your NVIDIA gpu is being used, see [this](https://unix.stackexchange.com/questions/16407/how-to-check-which-gpu-is-active-in-linux).

2. **docker**. If you have not updated your docker installation in a while, or don't have one, you can get the latest version of `docker-ce` for your OS from [here](https://docs.docker.com/install/).

3. **nvidia-docker**. This is needed to be able to run docker containers with the nvidia CUDA cudnn backends we need. Follow the installation instructions [here](https://github.com/NVIDIA/nvidia-docker).


## Getting started

1. Clone this repo and from the command line, switch to the repo directory.

2. From the repo directory run the following (and then get a coffee. Typically, the build process will take some time) :
```
nvidia-docker build --rm -t gpu-keras-tidyverse:1.0 gpu-keras-tidyverse
```
3. Run the container with
```
sudo nvidia-docker run --name deeplearning-r -d -p 8787:8787 -v ~/:/home/rstudio gpu-keras-tidyverse:1.0
```
4. In your browser, navigate to http://localhost:8787/

5. Login with rstudio:rstudio

6. Open the `keras_playground.Rmd` notebook from the repo directory and try it out !

## Known issues

 - While fitting a model if you get an error that looks like `TypeError: update() takes from 2 to 3 positional arguments but 4 were give`, [see this issue](https://github.com/rstudio/keras/issues/285). The fix is to run the following from within the container before working with keras.
```r
devtools::install_github('rstudio/keras')
keras::install_keras(tensorflow = 'gpu')
```
## Notes

 - once done with a session, stop the container with `nvidia-docker stop deeplearning-r`.
 - to start a session, `nvidia-docker start deeplearning-r` and navigate to http://localhost:8787/
