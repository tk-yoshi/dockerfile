FROM python:3.12-slim-bookworm AS python

FROM nvidia/cuda:12.6.2-cudnn-devel-ubuntu24.04 AS base


COPY --from=python /usr/local /tmp/local
RUN cp -a /tmp/local /usr\
    && rm -rf /tmp/local


ARG TORCH_VERSION=2.5.1
ARG VISION_VERSION=0.20.1
ARG CUDA_VERSION=124
RUN pip install torch==${TORCH_VERSION} torchvision==${VISION_VERSION} torchaudio==${TORCH_VERSION} --index-url https://download.pytorch.org/whl/cu${CUDA_VERSION}


RUN pip install\
    ipykernel\
    matplotlib\
    numpy\
    pandas\
    scipy


COPY matplotlibrc /root/.config/matplotlib/matplotlibrc

