# The Way of the Cyber Warrior
# Dockerfile: Threat Intel
# JupyterBook Dockerfile
# Author: Diego Perez (@darkquassar)
# License: GPL-3.0

FROM jupyter/minimal-notebook:python-3.9.13
LABEL maintainer="Diego Perez (@darkquassar)"
LABEL description="CyberWarrior Training Programme: Protect your Data Dockerfile"

# *** Setting up Env Variables ***
# Generic ENV
ENV THREATHUNTERZ_DIR /opt/threathunterz/
ENV THREATHUNTERZ_SCRIPTS /opt/threathunterz/scripts

# Preping Docker Image according to MyBinder instructions
# Ref: https://mybinder.readthedocs.io/en/latest/tutorials/dockerfile.html

ARG NB_USER
ARG NB_UID
ENV NB_USER jovyan
ENV NB_UID 1000
ENV NB_USER_HOME /home/${NB_USER}
ENV PATH "$NB_USER_HOME/.local/bin:$PATH"

USER root

RUN apt-get update --yes \
    && mkdir ${NB_USER_HOME}/thewayof \
    && mamba install --yes \
    # cyberpandas <-- check, not yet compatible with python 3.10
        'cyberpandas' \
        'jupyter-server-proxy' \
        'nbconvert' \
        'matplotlib' \
        'pandas' \
        's3contents' \
        's3fs' \
    # 2.1 - Let's add packages not available in conda-forge
    # 2.1.1 - Installing Threat Intelligence Packages
    && pip3 install holehe \
    && pip3 install pipx \
    && pipx ensurepath \
    && pipx install ghunt \
    # 2.2 - Installing JupyterLab Extensions
    && jupyter labextension install @jupyterlab/hub-extension \
    && jupyter labextension install @jupyter-widgets/jupyterlab-manager \
    && jupyter-labextension enable @jupyter-widgets/jupyterlab-manager \
    && jupyter labextension install jupyterlab-spreadsheet \
    # 2.3 - Running some cleanup commands
    && mamba clean --all -f -y \
    && fix-permissions "${CONDA_DIR}" \
    && fix-permissions "/home/${NB_USER}"

USER ${NB_USER}
COPY ./thewayof/ ${NB_USER_HOME}/thewayof

USER root
RUN chown -R ${NB_USER}:${NB_USER} ${NB_USER_HOME} ${JUPYTER_NOTEBOOKS_DIR}

WORKDIR ${NB_USER_HOME}
USER ${NB_USER}