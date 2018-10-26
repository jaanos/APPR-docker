# Slika `r-binder`

Slika `r-binder` je osnovna slika z okoljema [RStudio](http://jupyter.org/)
in [Jupyter](http://jupyter.org/) z [IRKernel](http://jupyter.org/)
za poganjanje na spletu z orodjem [Binder](https://mybinder.org/).

Konfiguracijska datoteka [`Dockerfile`](Dockerfile) je osnovana na tisti,
ki jo generira orodje [`repo2docker`](https://github.com/jupyter/repo2docker).
Tako slika vsebuje tudi strežnik [Shiny](https://shiny.rstudio.com/),
ne vsebuje pa R-jevega paketa `shiny` in ostalih dodatnih paketov.

Generiranje slike na [Docker Hub](https://hub.docker.com/) traja približno 30 do 45 minut.
