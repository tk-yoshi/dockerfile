FROM texlive/texlive:latest-full-doc-src AS base


RUN apt update -y\
    && apt upgrade -y\
    && apt install -y\
    libencode-locale-perl\
    python3.12-venv\
    && rm -r /var/lib/apt/lists/*

RUN python -m venv /adstex\
    && echo source /adstex/bin/activate >> ~/.bashrc

RUN . /adstex/bin/activate\
    && pip install adstex

RUN echo export ADS_API_TOKEN="S4EjmIgtCfQecVBWxRY7KPS2hDbYPDpmEStdZisb" >> ~/.bashrc
