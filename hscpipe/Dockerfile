FROM redhat/ubi8:8.10 AS builder

SHELL ["/bin/bash", "-c"]

RUN dnf update -y && dnf install -y\
    libxcb\
    git\
    patch\
    tk\
    wget\
    xz

RUN pushd /opt\
    && wget https://hscdata.mtk.nao.ac.jp/hsc_bin_dist/hscpipe/8.5.3/hscPipe-8.5.3-centos-8-x86_64.v4.tar.xz\
    && tar xvf hscPipe-8.5.3-centos-8-x86_64.v4.tar.xz\
    && rm hscPipe-8.5.3-centos-8-x86_64.v4.tar.xz\
    && wget https://hscdata.mtk.nao.ac.jp/hsc_bin_dist/patch-AddFilters-hscPipe-8.v5.sh\
    && bash patch-AddFilters-hscPipe-8.v5.sh /opt/hscpipe/8.5.3\
    && wget https://hscdata.mtk.nao.ac.jp/hsc_bin_dist/patch-FixTooManyOpenFiles-hscPipe-8.sh\
    && bash patch-FixTooManyOpenFiles-hscPipe-8.sh /opt/hscpipe/8.5.3


RUN pushd /opt/hscpipe\
    && git clone https://github.com/jcoupon/maskUtils.git

ARG MASKUTILS_DIR=/opt/hscpipe/maskUtils
RUN source /opt/hscpipe/8.5.3/bashrc\
    && setup-hscpipe\
    && setup -v -j -r ${MASKUTILS_DIR}\
    && scons -Q opt=3 -C ${MASKUTILS_DIR}

FROM redhat/ubi8:8.10 AS base

COPY --from=builder /opt /opt

RUN dnf update -y && dnf install -y\
    libxcb\
    tk

ARG MASKUTILS_DIR=/opt/hscpipe/maskUtils
RUN echo source /opt/hscpipe/8.5.3/bashrc >> ~/.bashrc\
    && echo setup-hscpipe >> ~/.bashrc\
    && echo setup -v -j -r ${MASKUTILS_DIR} >> ~/.bashrc


