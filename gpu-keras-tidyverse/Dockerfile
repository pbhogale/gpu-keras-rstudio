## Based on work by https://github.com/rocker-org/rocker/issues/273
## and particularly https://github.com/rocker-org/rocker/issues/273#issuecomment-349814504
##
## Based on Ubuntu 16.04

FROM nvidia/cuda:9.0-cudnn7-runtime

ENV CRAN_URL https://cloud.r-project.org/

RUN set -e \
      && apt-get -y update \
      && apt-get -y install libapparmor1 libcurl4-openssl-dev libxml2-dev libssl-dev gdebi-core apt-transport-https pandoc

RUN set -e \
      && grep '^DISTRIB_CODENAME' /etc/lsb-release \
        | cut -d = -f 2 \
        | xargs -I {} echo "deb ${CRAN_URL}bin/linux/ubuntu {}/" \
        | tee -a /etc/apt/sources.list \
      && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9 \
      && apt-get -y update \
      && apt-get -y upgrade \
      && apt-get install -y --no-install-recommends r-base \
      && apt-get install -y apt-utils \
      && apt-get -y autoremove \
      && apt-get clean



RUN set -e \
      && R -e "\
      update.packages(ask = FALSE, repos = '${CRAN_URL}'); \
      pkgs <- c('dbplyr', 'devtools', 'docopt', 'doParallel', 'foreach', 'gridExtra', 'rmarkdown', 'tidyverse'); \
      install.packages(pkgs = pkgs, dependencies = TRUE, repos = '${CRAN_URL}'); \
      sapply(pkgs, require, character.only = TRUE);"

RUN set -e \
      && apt-get install -y curl \
      && curl -sS https://s3.amazonaws.com/rstudio-server/current.ver \
        | xargs -I {} curl -sS http://download2.rstudio.org/rstudio-server-{}-amd64.deb -o /tmp/rstudio.deb \
      && gdebi -n /tmp/rstudio.deb \
      && rm -rf /tmp/*

RUN set -e \
      && useradd -m -d /home/rstudio rstudio \
      && echo rstudio:rstudio \
        | chpasswd

RUN set -e \
      && grep '^DISTRIB_CODENAME' /etc/lsb-release \
        | cut -d = -f 2 \
        | xargs -I {} echo "deb ${CRAN_URL}bin/linux/ubuntu {}/" \
        | tee -a /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9 \
	&& apt-get update \
    && apt-get upgrade -y -q \
    && apt-get install -y --no-install-recommends \
           r-base \
           r-base-dev \
           r-cran-littler \
           libxml2-dev \
           libxt-dev \
           libssl-dev \
           libcurl4-openssl-dev \
           imagemagick \
           python-pip \
           libpython2.7 \
    && pip install --upgrade pip \
    && pip install virtualenv \
    && echo 'options(repos = c(CRAN = "https://cloud.r-project.org"))' >> /etc/R/Rprofile.site \
    && /usr/lib/R/site-library/littler/examples/install.r tensorflow keras \
&& r -e "install.packages('devtools')" \
&& r -e "devtools::install_github('rstudio/keras')" \
&& r -e "keras::install_keras(tensorflow = 'gpu')"

EXPOSE 8787

CMD ["/usr/lib/rstudio-server/bin/rserver", "--server-daemonize=0", "--server-app-armor-enabled=0"]

