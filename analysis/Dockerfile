FROM python:3.12-slim-bookworm AS python

FROM texlive/texlive:latest-full-doc-src AS base

COPY --from=python /usr/local /tmp/local
RUN cp -a /tmp/local /usr\
    && rm -rf /tmp/local

RUN pip install\
    astropy\
    jupyterlab\
    matplotlib\
    numpy\
    pandas\
    scikit-learn\
    scipy\
    tensorflow\
    uncertainties

# COPY --from=tex /usr/local/texlive /usr/local/texlive

# RUN apt update -y && apt install -y\
#     perl-modules-5.36\
#     && rm -r /var/lib/apt/lists/*

# RUN /usr/local/texlive/2024/bin/x86_64-linux/tlmgr path add

COPY matplotlibrc /root/.config/matplotlib/matplotlibrc

