FROM bioconductor/devel_proteomics2

LABEL version="1.0"
LABEL description="This file illustrates \
how to compile CS code using the \
ThermoRawFileReadera library and use it \
as commandline programm."

MAINTAINER Christian Panse <Christian.Panse@gmail.com>

# download from http://planetorbitrap.com/rawfilereader#.WjkqIUtJmL4
ADD ThermoRawFileReader/ThermoFisher.CommonCore.RawFileReader.4.0.26.nupkg /tmp/

RUN apt-get update -y \
  && apt-get install mono-complete vim less unzip r-base curl -y \
  && cd /tmp/ \
  && unzip /tmp/ThermoFisher.CommonCore.RawFileReader.4.0.26.nupkg \
  && gacutil -i lib/Linux/ThermoFisher.CommonCore.BackgroundSubtraction.dll \
  && gacutil -i lib/Linux/ThermoFisher.CommonCore.Data.dll \
  && gacutil -i lib/Linux/ThermoFisher.CommonCore.RawFileReader.dll \
  && echo $?

ADD fgcz_raw.cs /tmp/
RUN cd /tmp \
  && mcs /out:/usr/local/bin/fgcz_raw.exe \
  fgcz_raw.cs /r:lib/Linux/ThermoFisher.CommonCore.Data.dll \
  /r:lib/Linux/ThermoFisher.CommonCore.MassPrecisionEstimator.dll \
  /r:lib/Linux/ThermoFisher.CommonCore.RawFileReader.dll /target:exe \
  && echo $?

# Install dependencies and Download and install shiny server
RUN apt-get update -y && apt-get install -y \
    sudo \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxml2 \
    libxml2-dev \
    libssl-dev \
    libxt-dev && \
    wget --no-verbose https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt" && \
    VERSION=$(cat version.txt)  && \
    wget --no-verbose "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb && \
    gdebi -n ss-latest.deb && \
    rm -f version.txt ss-latest.deb && \
    R -e "install.packages(c('shiny', 'rmarkdown'), repos='https://cran.rstudio.com/')" && \
    cp -R /usr/local/lib/R/site-library/shiny/examples/* /srv/shiny-server/ && \
    rm -rf /var/lib/apt/lists/*

EXPOSE 3838
COPY shiny-server.sh /usr/bin/shiny-server.sh
RUN chmod ugo+x /usr/bin/shiny-server.sh
CMD ["/usr/bin/shiny-server.sh"]

ADD install.R /tmp
RUN R --no-save < /tmp/install.R && echo $?
ADD installAddons.R /tmp
RUN R --no-save < /tmp/installAddons.R && echo $?
RUN mkdir -p /opt/rawdiag

COPY shiny/demo /opt/rawdiag
RUN chown -R shiny.shiny /opt/rawdiag
RUN mkdir -p /var/log/shiny-server
RUN mkdir -p /srv/shiny-server
RUN rm -r /srv/shiny-server/*
RUN ln -s /opt/rawdiag /srv/shiny-server/rawdiag
RUN rm -rf /var/lib/apt/lists/*
RUN mkdir -p /var/data/rawdiag
RUN chown -R shiny:shiny /var/data/rawdiag

