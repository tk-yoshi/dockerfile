FROM python:3.12.7-slim-bookworm AS builder

SHELL ["/bin/bash", "-c"]

RUN apt update\
    && apt install -y\
    tar\
    wget\
    && rm -r /var/lib/apt/lists/*

RUN pip install numpy==1.26.4\
    && pushd /tmp\
    && wget http://gitlab.lam.fr/cigale/cigale/-/archive/v2022.1/cigale-v2022.1.tar.gz\
    && tar xf cigale-v2022.1.tar.gz\
    && pushd cigale-v2022.1\
    && pip install .

FROM python:3.12.7-slim-bookworm AS base

COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin
