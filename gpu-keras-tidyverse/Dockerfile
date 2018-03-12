## see https://github.com/pbhogale/gpu-keras-rstudio

FROM nvidia/cuda:9.0-cudnn7-runtime


MAINTAINER "prasanna bhogale"

ENV CRAN_URL https://cloud.r-project.org/

RUN set -e \
	&& apt-get -y update \
    && apt-get -y install apt-utils libapparmor1 libcurl4-openssl-dev libxml2-dev libssl-dev gdebi-core apt-transport-https pandoc libssh2-1-dev libpython2.7 python-pip imagemagick libxt-dev libxml2-dev xterm

RUN set -e \
    && grep '^DISTRIB_CODENAME' /etc/lsb-release \
    | cut -d = -f 2 \
    | xargs -I {} echo "deb ${CRAN_URL}bin/linux/ubuntu {}/" \
    | tee -a /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9 \
    && apt-get -y update \
    && apt-get -y upgrade \
    && apt-get install -y --no-install-recommends \
    r-base \
    r-base-dev \
    r-cran-littler \
    && apt-get -y autoremove \
    && apt-get clean


RUN set -e \
    && apt-get install -y curl \
    && curl -sS https://s3.amazonaws.com/rstudio-server/current.ver \
    | xargs -I {} curl -sS http://download2.rstudio.org/rstudio-server-{}-amd64.deb -o /tmp/rstudio.deb \
    && gdebi -n /tmp/rstudio.deb \
    && rm -rf /tmp/* \
    && apt-get -y autoremove \
    && apt-get clean

RUN set -e \
    && useradd -m -d /home/rstudio rstudio \
    && echo rstudio:rstudio \
    | chpasswd \
    && apt-get -y autoremove \
    && apt-get clean

RUN set -e \
    && grep '^DISTRIB_CODENAME' /etc/lsb-release \
    | cut -d = -f 2 \
    | xargs -I {} echo "deb ${CRAN_URL}bin/linux/ubuntu {}/" \
    | tee -a /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9 \
	&& apt-get update \
    && apt-get upgrade -y -q \
    && pip install --upgrade pip \
    && pip install virtualenv \
    && rm -rf .cache \
    && echo 'options(repos = c(CRAN = "https://cloud.r-project.org"))' >> /etc/R/Rprofile.site \
    && /usr/lib/R/site-library/littler/examples/install.r tensorflow keras \
	&& r -e "install.packages(c('devtools', 'keras', 'xgboost', 'tidyverse', 'rmarkdown'))" \
	&& r -e "keras::install_keras(tensorflow = 'gpu')" \
    && apt-get -y autoremove \
    && apt-get clean

EXPOSE 8787

CMD ["/usr/lib/rstudio-server/bin/rserver", "--server-daemonize=0", "--server-app-armor-enabled=0"]

