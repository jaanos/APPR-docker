# Slika `base`

Slika `base` je osnovna slika, namenjena poganjanju projektov
pri predmetu Analiza podatkov s programom R.
Sloni na sliki [`r-binder`](../r-binder/), namesti pa večino paketov za R,
ki so potrebni za poganjanje projektov.
Med drugim slika ponuja funkcionalen strežnik [Shiny](https://shiny.rstudio.com/).

## Nastavitve

Seznam nameščenih paketov je v datoteki [`install.R`](install.R).

V datoteki [`Dockerfile`](Dockerfile) spremenljivka `MRAN_DATE` določa datum (v obliki `YYYY-MM-DD`),
za katerega se uporabi slika repozitorija [CRAN](https://cran.r-project.org/) paketov za R.
Tako je potrebno popraviti datum, če se nameščajo novejše različice paketov.

Generiranje slike na [Docker Hub](https://hub.docker.com/) traja približno 2 uri.
