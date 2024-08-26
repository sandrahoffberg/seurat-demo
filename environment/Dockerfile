# hash:sha256:39d525cd0b21fceeaf53b5b3b4936232f1dfd9c28ac52300fa2fb8f421da2afb
ARG REGISTRY_HOST
FROM $REGISTRY_HOST/codeocean/py-r:python3.10.12-R4.2.2-IRkernel-ubuntu22.04

ARG DEBIAN_FRONTEND=noninteractive

ARG GIT_ASKPASS
ARG GIT_ACCESS_TOKEN
COPY git-askpass /

RUN mamba install -y \
        r-irkernel==1.3.2 \
        r-rcpp==1.0.11 \
        r-seurat==5.1.0 \
    && mamba clean -ya

ARG RCACHE_OPTIONS
RUN echo $RCACHE_OPTIONS >> $(Rscript -e 'cat(R.home())')/etc/Rprofile.site
RUN Rscript -e 'remotes::install_version("BiocManager", "1.30.24")' \
    && Rscript -e 'remotes::install_version("markdown", "1.13")'

RUN Rscript -e 'remotes::install_github( \
        "immunogenomics/presto", \
        ref = "7636b3d0465c468c35853f82f1717d3a64b3c8f6")'

RUN Rscript -e 'options(warn=2); install.packages("BiocManager")'
RUN Rscript -e 'options(warn=2); BiocManager::install(c( \
        "BiocVersion", \
        "limma" \
    ))' # Original versions: 3.18.1 3.58.1
