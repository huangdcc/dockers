FROM rocker/r-base:4.4.1

RUN apt-get update --fix-missing -y
RUN apt-get install -y cmake
RUN apt-get install -y libcurl4-openssl-dev libssl-dev

## most recent commit
RUN wget https://github.com/shahcompbio/hmmcopy_utils/archive/29a8d1d.zip && unzip 29a8d1d.zip 
RUN cmake ./hmmcopy_utils-29a8d1d18dfd301600d5d91832e5fe231935058c && make

## BioC packages
RUN R -e "install.packages('BiocManager',dependencies=TRUE, repos='http://cran.rstudio.com/')"
RUN R -e "BiocManager::install(version = '3.19', ask = F)"
RUN R -e "BiocManager::install('HMMcopy', ask = F, version = '3.19')"
RUN R -e "BiocManager::install('GenomicRanges', ask = F,version = '3.19')"
RUN R -e "BiocManager::install('GenomeInfoDb', ask = F, version = '3.19')"
RUN apt-get install -y libxml2-dev
# libiconv.so.2: cannot open shared object file
RUN apt-get install -y libiconv-hook-dev 
RUN R -e "BiocManager::install('BSgenome.Hsapiens.UCSC.hg19', ask = F, version = '3.19')"
RUN R -e "BiocManager::install('BSgenome.Hsapiens.UCSC.hg38', ask = F,  version = '3.19')"
RUN R -e "install.packages('remotes', dependencies = TRUE)"
## CRAN packages
RUN R -e "install.packages(c('RCurl','plyr', 'foreach', 'data.table','doMC', 'ggplot2', 'strings'),dependencies=TRUE, repos='http://cran.rstudio.com/')"

# Install from github
RUN R -e "remotes::install_github('GavinHaLab/ichorCNA@v0.6.0', dependencies = TRUE)"
# Download support data
RUN wget https://github.com/GavinHaLab/ichorCNA/archive/refs/tags/v0.6.0.zip && unzip v0.6.0.zip && mv ichorCNA-0.6.0/ ichorCNA/

# Override the R CMD for workflow managers
CMD ["/bin/bash"]