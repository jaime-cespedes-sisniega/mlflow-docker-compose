FROM python:3.8-slim

ENV MLFLOW_VERSION 1.23.0
ENV PSYCOPG_VERSION 2.9.3
ENV BOTO3_VERSION 1.20.41

RUN pip install --no-cache-dir mlflow==${MLFLOW_VERSION} \
                               psycopg2-binary==${PSYCOPG_VERSION} \
                               boto3==${BOTO3_VERSION}