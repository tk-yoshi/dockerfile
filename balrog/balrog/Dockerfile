FROM python:2.7.18-slim-buster AS builder

SHELL ["/bin/bash", "-c"]

COPY sources.list /etc/apt/sources.list

RUN apt update\
    && apt install -y\
    # libatlas-base-dev\
    libboost-python-dev\
    libfftw3-dev\
    psfex\
    scons\
    sextractor\
    wget\
    g++\
    git\
    && rm -r /var/lib/apt/lists/*

# RUN mkdir /opt/scons\
#     && pushd /opt/scons\
#     && wget https://sourceforge.net/projects/scons/files/scons/2.1.0/scons-2.1.0.tar.gz\
#     && tar xf scons-2.1.0.tar.gz\
#     && pushd scons-2.1.0\
#     && python setup.py install\
#     && popd\
#     && rm scons-2.1.0.tar.gz


RUN mkdir /opt/tmv\
    && pushd /opt/tmv\
    && wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/tmv-cpp/tmv0.71.tar.gz\
    && tar xf tmv0.71.tar.gz\
    && pushd tmv0.71\
    && scons install\
    && popd\
    && rm tmv0.71.tar.gz


# RUN mkdir /opt/boost\
#     && pushd /opt/boost\
#     && wget https://sourceforge.net/projects/boost/files/boost/1.40.0/boost_1_40_0.tar.gz\
#     && tar xf boost_1_40_0.tar.gz\
#     && pushd boost_1_40_0\
#     && ./bootstrap.sh\
#     && ./bjam -j8 install; exit 0\
#     && popd\
#     && rm boost_1_40_0.tar.gz


# RUN mkdir /opt/fftw\
#     && pushd /opt/fftw\
#     && wget https://www.fftw.org/fftw-3.3.10.tar.gz\
#     && tar xf fftw-3.3.10.tar.gz\
#     && pushd fftw-3.3.10\
#     && ./configure\
#         --enable-float\
#     && make -j8\
#     && make install\
#     && ./configure\
#     && make -j8\
#     && make install\
#     && ./configure\
#         --enable-long-double\
#     && make -j8\
#     && make install


# RUN pushd /opt\
#     && wget https://sourceforge.net/projects/math-atlas/files/Stable/3.10.3/atlas3.10.3.tar.bz2\
#     && tar xf atlas3.10.3.tar.bz2\
#     && pushd ATLAS\
#     && mkdir build\
#     && cd build\
#     && ../configure\
#     && make -j8\
#     && make install

# RUN mkdir /opt/sextractor\
#     && pushd /opt/sextractor\
#     && wget https://github.com/astromatic/sextractor/archive/refs/tags/2.25.0.tar.gz\
#     && tar xf 2.25.0.tar.gz\
#     && pushd sextractor-2.25.0\
#     && ./autogen.sh\
#     && ./configure\
#     && make -j8\
#     && make install


# RUN mkdir /opt/psfex\
#     && pushd /opt/psfex\
#     && wget https://github.com/astromatic/psfex/archive/refs/tags/3.18.2.tar.gz\
#     && tar xf 3.18.2.tar.gz\
#     && pushd psfex-3.18.2\
#     && ./autogen.sh\
#     && ./configure\
#     && make -j8\
#     && make install


RUN pip install\
    numpy\
    astropy\
    pyyaml\
    scipy\
    && pip install starlink-pyast


RUN mkdir /opt/galsim\
    && pushd /opt/galsim\
    && wget https://github.com/GalSim-developers/GalSim/archive/v1.1.1.tar.gz\
    && tar xf v1.1.1.tar.gz\
    && pushd GalSim-1.1.1\
    && scons\
    && scons PYPREFIX=/usr/local/lib/python2.7/site-packages install\
    && popd\
    && rm v1.1.1.tar.gz


RUN ln -s /usr/bin/sextractor /usr/local/bin/sex

RUN pushd /opt\
    && git clone https://github.com/emhuff/Balrog.git\
    && pushd Balrog\
    && sed -i 's/-bad/~bad/g' /opt/Balrog/model_class.py\
    && sed -i 's/BalrogSeutp/BalrogSetup/g' /opt/Balrog/balrog.py\
    && ./balrog.py --fulltraceback


FROM python:2.7.18-slim-buster AS base

COPY sources.list /etc/apt/sources.list

RUN apt update\
    && apt install -y\
    libboost-python1.*.0\
    libfftw3-double3\
    libfftw3-long3\
    libfftw3-single3\
    psfex\
    sextractor\
    && rm -r /var/lib/apt/lists/*

RUN ln -s /usr/bin/sextractor /usr/local/bin/sex


COPY --from=builder /usr/local/lib /usr/local/lib

COPY --from=builder /opt/Balrog /opt/Balrog

CMD ["bash"]
