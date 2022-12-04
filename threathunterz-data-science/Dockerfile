# The Way of the Cyber Warrior
# Dockerfile: Data Science
# JupyterBook Dockerfile
# Author: Diego Perez (@darkquassar)
# License: GPL-3.0

FROM jupyter/minimal-notebook:2022-11-28
LABEL maintainer="Diego Perez (@darkquassar)"
LABEL description="The Way of the Cyber Warrior Dockerfile"

# *** Setting up Env Variables ***
# Generic ENV
ENV THREATHUNTERZ_DIR /opt/threathunterz/
ENV THREATHUNTERZ_SCRIPTS /opt/threathunterz/scripts

# Conda & Jupyter ENV
#ENV ANACONDA_DIR /opt/threathunterz/conda3
#ENV JUPYTER_DIR /opt/threathunterz/jupyter
#ENV JUPYTER_NOTEBOOKS_DIR /opt/threathunterz/jupyter/notebooks
#ENV PATH $ANACONDA_DIR/bin:$PATH

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
        'altair' \
        # cyberpandas <-- check, not yet compatible with python 3.10
        #'cyberpandas' \
        'dask' \
        'elasticsearch-dsl' \
        'jupytext' \
        'jupyter-server-proxy' \
        'kafkacat' \
        'kafka-python' \
        'koalas' \
        'nbconvert' \
        'matplotlib' \
        'pandas' \
        'pika' \
        'seaborn' \
        's3contents' \
        's3fs' \
    # 2.1 - Let's add packages not available in conda-forge
    && python3 -m pip install ksql \
    # && python3 -m pip install nbreport \ --> need to test, not working
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