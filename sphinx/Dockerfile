FROM python:3.13-slim-bookworm AS base

RUN pip install\
    pydata-sphinx-theme\
    sphinx\
    sphinx-copybutton\
    sphinx-design\
    sphinx-toggleprompt\
    sphinx-toolbox

RUN apt update -y && apt install -y\
    git\
    make\
    && rm -r /var/lib/apt/lists/*

