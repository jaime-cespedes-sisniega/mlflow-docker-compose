# MLflow - Docker Compose
This repository aims to use a MLflow server for experiment tracking or as a model registry using PostgreSQL for the metadata storage and MinIO for artifacts storage. In addition, access to the MLflow server requires authentication through the use of NGINX.

## Requirements

The following docker compose repository has been tested using:
* [Docker Engine 20.10.12](https://docs.docker.com/engine/release-notes/#201012)
* [Docker Compose 1.29.2](https://docs.docker.com/compose/release-notes/#1292)
* Ubuntu 20.04.3 LTS

## Setup

Modify and complete `.env.example` file to set your PostgreSQL, MinIO, MLflow, SSL/TSL certifications and configuration parameters.

Rename `.env.example` to `.env`.

```bash
mv .env.example .env
```

In order to enable basic authentication to access the MLflow server, NGINX is used. To create username-password pairs, [Configuring http basic authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/) guide from NGINX documentation can be followed. The path where the user's information (username:encrypted-password) is stored is the same as the one used in the guide `/etc/apache2/.htpasswd`.

## Usage

When using authentication from MLflow to log experiments or models, username and password (unencrypted) need to be set using the following environment variables. 

> **_NOTE_**: Both the user and password must be the same as those created previously to use NGINX authentication. In addition, `MLFLOW_S3_ENDPOINT_URL` (host where MinIO service is running), `AWS_ACCESS_KEY_ID` (MinIO username), `AWS_SECRET_ACCESS_KEY` (MinIO password) and `AWS_CA_BUNDLE` (MinIO CA bundle) must be provided.

```python
import os
...
os.environ['MLFLOW_TRACKING_USERNAME'] = <NGINX_USERNAME>
os.environ['MLFLOW_TRACKING_PASSWORD'] = <NGINX_PASSWORD_UNENCRYPTED>
os.environ['MLFLOW_TRACKING_INSECURE_TLS'] = 'false'
os.environ['MLFLOW_TRACKING_SERVER_CERT_PATH'] = <MLFLOW_CA_BUNDLE>
os.environ['MLFLOW_S3_ENDPOINT_URL'] = <MINIO_HOST>
os.environ['AWS_ACCESS_KEY_ID'] = <MINIO_MLFLOW_USER>
os.environ['AWS_SECRET_ACCESS_KEY'] = <MINIO_MLFLOW_PASSWORD>
os.environ['AWS_CA_BUNDLE'] = <MINIO_CA_BUNDLE>
```
More information about authentication can be found in the MLflow´s documentation [Logging to a Tracking Server](https://mlflow.org/docs/latest/tracking.html#logging-to-a-tracking-server).


Docker compose services can be executed with the following command.
```bash
docker-compose up -d
```

Finally, you can access and login to the MLflow server through [http://127.0.0.1](http://127.0.0.1) and also access MinIO server through [http://127.0.0.1:9000](http://127.0.0.1:9000). 