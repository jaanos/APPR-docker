FROM buildpack-deps:bionic

# avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Set up locales properly
RUN apt-get update && \
    apt-get install --yes --no-install-recommends locales && \
    apt-get purge && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Use bash as default shell, rather than sh
ENV SHELL /bin/bash

# Set up user
ENV NB_USER jovyan
ENV NB_UID 1000
ENV USER ${NB_USER}
ENV HOME /home/${NB_USER}

RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}
WORKDIR ${HOME}

RUN wget --quiet -O - https://deb.nodesource.com/gpgkey/nodesource.gpg.key |  apt-key add - && \
    DISTRO="bionic" && \
    echo "deb https://deb.nodesource.com/node_10.x $DISTRO main" >> /etc/apt/sources.list.d/nodesource.list && \
    echo "deb-src https://deb.nodesource.com/node_10.x $DISTRO main" >> /etc/apt/sources.list.d/nodesource.list

RUN apt-get update && \
    apt-get install --yes --no-install-recommends \
       less \
       nodejs \
       unzip && \
    apt-get install --yes \
       libapparmor1 \
       lsb-release \
       psmisc \
       r-base \
       sudo \
       cargo \
       libgdal-dev \
       libgeos-dev \
       libgeos++-dev \
       libprotobuf-dev \
       && apt-get purge && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
EXPOSE 8888

# Environment variables required for build
ENV APP_BASE /srv
ENV CONDA_DIR ${APP_BASE}/conda
ENV NB_PYTHON_PREFIX ${CONDA_DIR}
ENV KERNEL_PYTHON_PREFIX ${NB_PYTHON_PREFIX}
ENV R_LIBS_USER ${APP_BASE}/rlibs
# Special case PATH
ENV PATH ${CONDA_DIR}/bin:$HOME/.local/bin:/usr/lib/rstudio-server/bin/:${PATH}
# If scripts required during build are present, copy them

COPY conda/environment.py-3.6.frozen.yml /tmp/environment.yml

COPY conda/install-miniconda.bash /tmp/install-miniconda.bash
RUN bash /tmp/install-miniconda.bash && \
    rm /tmp/install-miniconda.bash /tmp/environment.yml

RUN mkdir -p ${R_LIBS_USER} && \
    chown -R ${NB_USER}:${NB_USER} ${R_LIBS_USER}

RUN curl --silent --location --fail https://download2.rstudio.org/rstudio-server-1.1.419-amd64.deb > /tmp/rstudio.deb && \
    echo '24cd11f0405d8372b4168fc9956e0386 /tmp/rstudio.deb' | md5sum -c - && \
    dpkg -i /tmp/rstudio.deb && \
    rm /tmp/rstudio.deb

RUN curl --silent --location --fail https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.7.907-amd64.deb > /tmp/shiny.deb && \
    echo '78371a8361ba0e7fec44edd2b8e425ac /tmp/shiny.deb' | md5sum -c - && \
    dpkg -i /tmp/shiny.deb && \
    rm /tmp/shiny.deb

RUN sed -i -e '/^R_LIBS_USER=/s/^/#/' /etc/R/Renviron && \
    echo "R_LIBS_USER=${R_LIBS_USER}" >> /etc/R/Renviron

USER ${NB_USER}
RUN pip install --no-cache-dir nbrsessionproxy==0.7.0 && \
    jupyter serverextension enable nbrsessionproxy --sys-prefix && \
    jupyter nbextension install --py nbrsessionproxy --sys-prefix && \
    jupyter nbextension enable --py nbrsessionproxy --sys-prefix

RUN R --quiet -e "install.packages('devtools', repos='https://mran.microsoft.com/snapshot/2018-02-01', method='libcurl')" && \
    R --quiet -e "devtools::install_github('IRkernel/IRkernel', ref='0.8.11')" && \
    R --quiet -e "IRkernel::installspec(prefix='$NB_PYTHON_PREFIX')"

USER ${NB_USER}
CMD ["jupyter", "notebook", "--ip", "0.0.0.0"]
