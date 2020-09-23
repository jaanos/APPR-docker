FROM jaanos/appr:r-binder-2020

ENV MRAN_DATE 2020-09-21

USER root
COPY install.R ${HOME}
RUN chown -R ${NB_USER}:${NB_USER} ${HOME}

RUN echo "options(repos = c(CRAN='https://mran.microsoft.com/snapshot/${MRAN_DATE}'), download.file.method = 'libcurl')" > /etc/R/Rprofile.site

RUN install -o ${NB_USER} -g ${NB_USER} -d /var/log/shiny-server && \
    install -o ${NB_USER} -g ${NB_USER} -d /var/lib/shiny-server && \
    install -o ${NB_USER} -g ${NB_USER} /dev/null /var/log/shiny-server.log && \
    install -o ${NB_USER} -g ${NB_USER} /dev/null /var/run/shiny-server.pid

USER ${NB_USER}
RUN Rscript ${HOME}/install.R && rm ${HOME}/install.R
