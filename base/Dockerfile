ARG BASE_CONTAINER=dnbcreditriskprod.azurecr.io/conda/miniforge3:2025.01.15-1
FROM $BASE_CONTAINER

ARG release

SHELL ["/bin/bash", "-c"]

ENV PATH /opt/conda/bin:$PATH
ENV DASK_VERSION=${release:-2024.12.1}

RUN mamba install -y \
    "mamba>=0.27.0" \
    nomkl \
    cmake \
    dask=${DASK_VERSION} \
    cachey \
    streamz \
    && mamba clean -tipy \
    && find /opt/conda/ -type f,l -name '*.a' -delete \
    && find /opt/conda/ -type f,l -name '*.pyc' -delete \
    && find /opt/conda/ -type f,l -name '*.js.map' -delete \
    && find /opt/conda/lib/python*/site-packages/bokeh/server/static -type f,l -name '*.js' -not -name '*.min.js' -delete \
    && rm -rf /opt/conda/pkgs \
    && mamba clean -afy

COPY prepare.sh /usr/bin/prepare.sh
RUN chmod +x /usr/bin/prepare.sh

RUN mkdir /opt/app

ENTRYPOINT ["tini", "-g", "--", "/usr/bin/prepare.sh"]
