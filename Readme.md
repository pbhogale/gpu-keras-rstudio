# Dockerfile for GPU enabled rstudio server with keras+tensorflow

Use the dockerfile in this branch if you don't have an NVIDIA GPU available to you, but still want an ML environment setup with RStudio and keras and tensorflow and other useful packages.

**Disclaimer :** The Dockerfile needs re-factoring to be more efficient during the build process.
**Inspired by** https://github.com/rocker-org/ml/.


There is also an `.Rmd` file with some code snippets from the book, so that you can test if the setup works.


## Pre-requisites


1. **docker**. If you have not updated your docker installation in a while, or don't have one, you can get the latest version of `docker-ce` for your OS from [here](https://docs.docker.com/install/).


## Getting started

1. Clone this repo and from the command line, switch to the repo directory.

2. From the repo directory run the following (and then get a coffee. Typically, the build process will take some time) :
```
sudo docker build --rm -t gpu-keras-tidyverse:no-cuda gpu-keras-tidyverse
```
3. Run the container with
```
sudo docker run --name deeplearning-r-no-cuda -d -p 8789:8787 -v ~/:/home/rstudio gpu-keras-tidyverse:no-cuda
```
4. In your browser, navigate to http://localhost:8789/

5. Login with rstudio:rstudio

6. Open the `keras_playground.Rmd` notebook from the repo directory and try it out !

## Known issues

- In case an error of the type `Failed to load the native TensorFlow runtime.` turns up the first time you try to use keras, from the notebook just run
```r
library(keras)
install_keras()
library(keras)
```

## Notes

 - once done with a session, stop the container with `docker stop deeplearning-r-no-cuda`.
 - to start a session, `docker start deeplearning-r-no-cuda` and navigate to http://localhost:8787/
