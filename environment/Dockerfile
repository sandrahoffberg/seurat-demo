# hash:sha256:d5672c6bf0ecfc3d42d6991b91a342cbc4db80d136f860e78ce2fc5cf014fb4b
ARG REGISTRY_HOST
FROM $REGISTRY_HOST/codeocean/py-r:python3.10.12-R4.2.2-IRkernel-ubuntu22.04

ARG DEBIAN_FRONTEND=noninteractive

ARG GIT_ASKPASS
ARG GIT_ACCESS_TOKEN
COPY git-askpass /

RUN mamba install -y \
        r-irkernel \
        r-rcpp \
        r-seurat \
    && mamba clean -ya

ARG RCACHE_OPTIONS
RUN echo $RCACHE_OPTIONS >> $(Rscript -e 'cat(R.home())')/etc/Rprofile.site
RUN Rscript -e 'remotes::install_version("BiocManager")' \
    && Rscript -e 'remotes::install_version("markdown")'

RUN Rscript -e 'remotes::install_github( \
        "immunogenomics/presto")'

RUN Rscript -e 'options(warn=2); install.packages("BiocManager")'
RUN Rscript -e 'options(warn=2); BiocManager::install(c( \
        "BiocVersion", \
        "limma" \
    ))' # Original versions: latest latest
