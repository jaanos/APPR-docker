# Konfiguracije za Docker za potrebe APPR

* [![Jupyter](http://mybinder.org/badge.svg)](http://mybinder.org/v2/gh/jaanos/APPR-docker/master) Jupyter
* [![RStudio](http://mybinder.org/badge.svg)](http://mybinder.org/v2/gh/jaanos/APPR-docker/master?urlpath=rstudio) RStudio

Ta repozitorij vsebuje konfiguracijske datoteke za [Docker](https://shiny.rstudio.com/) za poganjanje projektov pri predmetu Analiza podatkov s programom R na spletu v orodju [Binder](https://mybinder.org/) z okoljema [RStudio](https://www.rstudio.com/) in [Shiny](https://shiny.rstudio.com/).

## Vsebina

Trenutno so na voljo konfiguracije za tri slike:

* [`r-binder`](r-binder/): osnovna slika z RStudiom in [IRKernel](https://irkernel.github.io/)
* [`base`](base/): slika za osnovno uporabo s Shiny in drugimi paketi
* [`extended`](extended/): slika z namestitvijo dodatnih paketov

Same slike gostujejo na [Docker Hub](https://hub.docker.com/) (in se tam tudi generirajo) v repozitoriju [`jaanos/appr`](https://hub.docker.com/r/jaanos/appr/). Aktualne slike imajo ime, ki se konča z `-2019`, vmesne slike pa imajo na koncu celoten datum. Slike brez priponke k imenu so iz 2018/19.

## Uporaba

Za poganjanje programa z repozitorija na GitHubu je najprej potrebno poskrbeti za datoteko `Dockerfile` v vrhnji mapi repozitorija, ki naj izgleda približno tako:

```Dockerfile
FROM jaanos/appr:base-2019

USER root
COPY . ${HOME}
RUN chown -R ${NB_USER} ${HOME}
USER ${NB_USER}

RUN if [ -f install.R ]; then R --quiet -f install.R; fi
```

Prva vrstica določa, katera slika (`base-2019`) iz katerega repozitorija (`jaanos/appr`) se bo uporabila. Sledijo vrstice, ki poskrbijo za kopiranje vsebine repozitorija v končno sliko. Zadnja vrstica preveri, ali obstaja datoteka `install.R`, in jo v tem primeru požene - tako je mogoče namestiti dodatne pakete.

Repozitorij z zgornjo datoteko je mogoče zagnati na naslovu

    http://mybinder.org/v2/gh/jaanos/APPR-docker/master

Namesto `jaanos/APPR-docker` seveda uporabimo ime želenega repozitorija, namesto `master` pa lahko podamo želeno ime veje ali značke oziroma šifro commita.
Privzeto se zažene Jupyter, ki nam omogoča delo tako s Python 3 kot z R. Pod *New* lahko izberemo tudi *RStudio Session*, kar bo pognalo RStudio v brskalniku.

Lahko pa tudi neposredno poženemo RStudio oziroma aplikacijo Shiny:

    http://mybinder.org/v2/gh/jaanos/APPR-docker/master?urlpath=rstudio
    http://mybinder.org/v2/gh/jaanos/APPR-docker/master?urlpath=shiny/pot/do/aplikacije

V slednjem primeru nadomestimo `pod/do/aplikacije` s potjo do mape z `ui.R` in `server.R` oziroma datoteke `.Rmd`.

## Prirejanje slik

S spodnjim postopkom lahko priredite slike svojim potrebam.

1.  S klikom na **Fork** desno zgoraj naredite lastno kopijo tega repozitorija.
2.  Prijavite se na [Docker Hub](https://hub.docker.com/).
3.  Desno zgoraj kliknite na svoje uporabniško ime, izberite *Settings*, nato pa v zgornji vrstici še *Linked Accounts & Services*.
4.  Kliknite na **Link Github** in nato potrdite, da ima Docker Hub zahtevane pravice dostopa.
5.  V zgornji vrstici kliknite na **Create**, izberite *Create Automated Build*, potem pa kliknite na **Create Auto-build Github**
6.  Iz seznama repozitorijev izberite kopijo tega repozitorija (`APPR-docker`).
7.  Nastavite ime repozitorija na Docker Hubu (npr. `appr`) in napišite kratek opis, nato pa kliknite na *Click here to customize*.
8.  V spodnji tabeli pod *Name* napišite ime veje, na katero naj push sproži generiranje slike (če boste prirejali samo eno sliko, je to lahko kar privzeta veja `master`), pod *Dockerfile Location* pot do `Dockerfile` za sliko, ki se prireja (npr `/extended/`), pod *Docker Tag* pa ime slike (npr. `extended`). Po potrebi lahko s klikom na **+** dodaste novo vrstico za novo sliko.
9.  Kliknite na *Create*, da shranite nastavitve.
10. Uredite nastavitvene datoteke slike, ki jo prirejate, in spremembe postavite na ustrezno vejo. Tako se bo sprožilo generiranje slike.
11. Na svojem repozitoriju na Docker Hubu lahko pod *Build Details* spremljate status generiranja slike.
12. Ko je slika uspešno generirana, lahko v prvi vrstici `Dockerfile` na repozitoriju svojega projekta nastavite, da se uporabi pravkar ustvarjena slika z vašega repozitorija.

Tako se bo vaš projekt poganjal s prirejeno sliko. Če ste v njej že namestili potrebne pakete, jih seveda ne nameščajte še enkrat z `install.R` v repozitoriju projekta.
