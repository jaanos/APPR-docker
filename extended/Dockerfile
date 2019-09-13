FROM jaanos/appr:base-2019

USER root
COPY install.R ${HOME}
RUN chown ${NB_USER}:${NB_USER} ${HOME}/install.R

USER ${NB_USER}
RUN Rscript ${HOME}/install.R && rm ${HOME}/install.R
