# rawDiag <img src="https://user-images.githubusercontent.com/12233339/39515832-84b561ea-4dfb-11e8-9411-276bc6fb71d6.png" align="right" width="100px" />

an R package supporting rational LC-MS method optimization for bottom-up proteomics on multiple OS platforms



## 1 System Requirements  

a Windows/Linux/MacOSX x64 platform 


### 1.1 .NET Framework and R

- https://www.mono-project.com/ (>4.0.22) for (Linux and MacOSX)
- .NET Framework 4.5.1 or higher (Windows)
- R (>3.4.0)
- install https://CRAN.R-project.org/package=devtools
- if you want support for [Open File Standards](http://www.psidev.info/) install the [mzR](http://bioconductor.org/packages/mzR/) package. 

### 1.2 The New RawFileReader .Net assembly from Thermo Fisher Scientific

**If your installation does not work with the below-mentioned instructions, do not hesitate to request a ready to run R package from the authors via Email, subject `request rawDiag package`.**


Due to licensing reasons, we currently not allowed to distribute Thermo Fisher Scientific software with the *rawDiag* package (we hope that this will change soon).
The [New RawFileReader from Thermo Fisher Scientific](http://planetorbitrap.com/rawfilereader)
has to be downloaded and installed separately in order to be able to directly read Thermo raw-files (by using the R function `read.raw`).

To install [the New RawFileReader .Net assembly](http://planetorbitrap.com/rawfilereader) follow the installation instructions provided by Thermo Fisher Scientific.


### 1.3 Platforms and versions the software has been tested on

The package has been tested on the following systems using R in R Studio:

|platform|platform version|R version|note|
| :------- |---------------:| -------:|:------- |
|Linux| Debian 8 (jessie) |  3.4.3 | [Demo system](http://fgcz-ms-shiny.uzh.ch:8080/bfabric_rawDiag/)|
|Linux     | Debian 10 ([buster](https://www.debian.org/releases/testing/releasenotes)) | 3.5.0 | CP |
|Linux| bioconductor/devel_proteomics2| 2017-12-31 r73996 | [dockerhub](https://hub.docker.com/r/cpanse/rawdiag/builds/) no RawFileReader support |
|Windows   | 7 x64| 3.4.1 |CT|
|Windows   | 10 x64| 3.4.4 |CP virtual box|
|Windows   | Server 2012 R2 x64 | 3.4.4|CP|
|Windows   | 10 x64 | 3.4.3 | WEW |
|Windows   | 10 x64 | R Open 3.5.0 | WEW |
|MacOSX    | 10.13.5 (17F77)|3.4.2|CP|
|MacOSX    | 10.11.6 (15G20015)|3.4.3 |JG|
|MacOSX    | 10.13.4 (17E202)|3.4.4|CP|

## 2 Installation guide

### 2.1 Instructions
To ensure the proper function of this R package please check if all the [requirements](README.md#1-system-requirements) are fullfilled prior to using it.

#### all OS

the following code downloads and installs the R package from the Github without the required third party .dll files:

please note: due to the data size (>=40MB) download can take a while
```{r}
# install.packages("devtools")
library("devtools")
devtools::install_github("fgcz/rawDiag", build_vignettes = FALSE)
```


### 2.2 Typical install time on a "normal" desktop computer

* Thermo RawFileReader dll: 1sec to 30 minutes
* the rawDiag package through github: 10 minutes 

## 3 Demonstration

### 3.1 R commandline code snippet

"Hello; World!" example on the R command line

```{r}
library(rawDiag)
data(WU163763)
PlotScanFrequency(WU163763, method='overlay')
PlotPrecursorHeatmap(WU163763)
PlotMassDistribution(WU163763
```

### 3.2 An interactive shiny example

#### in your local R shell
```{r}
# install.packages("shiny")
# install.packages("DT")
library(shiny)
rawDiag_shiny <- system.file('shiny', 'demo', package = 'rawDiag')
shiny::runApp(rawDiag_shiny, display.mode = 'normal')
```

#### using the docker image

source: [dockerhub](https://hub.docker.com/r/cpanse/rawdiag/)

```
docker pull cpanse/rawdiag \
&& docker run -it -p 8787:8787 cpanse/rawdiag R -e "library(shiny); \
   rawDiag_shiny <- system.file('shiny', 'demo', package = 'rawDiag'); \
   shiny::runApp(rawDiag_shiny, display.mode = 'normal', port=8787, host='0.0.0.0')"
```

connect with your web browser to `http://yourdockerhostname:8787`

### 3.3 using the `read.raw` method

taken from the `?read.raw` man page.
```{r}
(rawfile <- file.path(path.package(package = 'rawDiag'), 'extdata', 'sample.raw'))
system.time(RAW <- read.raw(file = rawfile))
 
summary.rawDiag(RAW)
PlotScanFrequency(RAW)
     
dim(RAW)
# now  read all dimensions
RAW <- read.raw(file = rawfile, rawDiag = FALSE)
dim(RAW)
```

## 4 Instructions for use

read the vignettes.

```{r}
browseVignettes('rawDiag')
```

the documentation of the function is available through the R man pages.

## 5 Useful links
- http://planetorbitrap.com/rawfilereader
- [screen recording (3:02 minutes, size 47MB, no audio track)](http://fgcz-ms.uzh.ch/~cpanse/PAPERS/pr-2018-001736.mov)
- [shiny demo on our compute server](http://fgcz-ms-shiny.uzh.ch:8080/rawDiag-demo/)
- *rawDiag - an R package supporting rational LC-MS method optimization for bottom-up proteomics*
Christian Trachsel, Christian Panse, Tobias Kockmann, Witold Eryk Wolski, Jonas Grossmann, Ralph Schlapbach
bioRxiv 304485; doi: https://doi.org/10.1101/304485
(manuscript submitted to Journal of Proteome Research; **pr-2018-001736**).

- [MassIVE MSV000082389](https://massive.ucsd.edu/ProteoSAFe/dataset.jsp?task=b231e78d674345798ebe50e46a9a3a93)

- [ASMS 2018 poster as PDF(1.8M, md5=dab9388c1a465d931e9d2345119a2827)](http://fgcz-ms.uzh.ch/~cpanse/ASMS2018_ID291250.pdf)
