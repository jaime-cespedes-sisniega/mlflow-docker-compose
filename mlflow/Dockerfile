FROM python:3.8-slim

RUN apt-get update && \
    apt-get install -y \
        openssh-server

RUN mkdir ~/.ssh/ && \
    touch ~/.ssh/known_hosts

ENV MLFLOW_VERSION 1.23.0
ENV PSYCOPG_VERSION 2.9.3
ENV PYSFTP_VERSION 0.2.9

RUN pip install --no-cache-dir mlflow==${MLFLOW_VERSION} \
                               psycopg2-binary==${PSYCOPG_VERSION} \
                               pysftp==${PYSFTP_VERSION}