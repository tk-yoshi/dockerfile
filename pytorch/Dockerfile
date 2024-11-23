FROM python:3.12.7-slim-bookworm AS python

FROM nvidia/cuda:12.6.1-cudnn-devel-ubuntu24.04 AS base


RUN apt update -y && apt install -y\
    ca-certificates \
    netbase \
    tzdata \
    && rm -r /var/lib/apt/lists/*


COPY --from=python /usr/local /tmp/local
RUN cp -a /tmp/local /usr\
    && rm -rf /tmp/local


RUN savedAptMark="$(apt-mark showmanual)"\
    && apt update\
    && apt install -y --no-install-recommends \
        dpkg-dev \
        gcc \
        gnupg \
        libbluetooth-dev \
        libbz2-dev \
        libc6-dev \
        libdb-dev \
        libexpat1-dev \
        libffi-dev \
        libgdbm-dev \
        liblzma-dev \
        libncursesw5-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl-dev \
        make \
        tk-dev \
        uuid-dev \
        wget \
        xz-utils \
        zlib1g-dev\
    && apt-mark auto '.*' > /dev/null\
    && apt-mark manual $savedAptMark\
    && find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' \
        | awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); printf "*%s\n", so }' \
        | sort -u \
        | xargs -r dpkg-query --search \
        | grep amd64\
        | cut -d: -f1 \
        | sort -u \
        | xargs -r apt-mark manual \
    && apt purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false\
    && rm -rf /var/lib/apt/lists/*


ARG TORCH_VERSION=2.4.1
ARG VISION_VERSION=0.19.1
ARG CUDA_VERSION=124
RUN pip install torch==${TORCH_VERSION} torchvision==${VISION_VERSION} torchaudio==${TORCH_VERSION} --index-url https://download.pytorch.org/whl/cu${CUDA_VERSION}

