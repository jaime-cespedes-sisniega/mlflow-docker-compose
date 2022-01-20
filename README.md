# MLflow - Docker Compose
This repository aims to use a MLflow server for experiment tracking or as a model registry. In addition, access to the MLflow server requires authentication through the use of NGINX.

## Requirements

The following docker compose repository has been tested using:
* [Docker Engine 20.10.12](https://docs.docker.com/engine/release-notes/#201012)
* [Docker Compose 1.29.2](https://docs.docker.com/compose/release-notes/#1292)
* Ubuntu 20.04.3 LTS

## Usage

Modify and complete `.env.example` file to set your PostgreSQL and MLflow configuration parameters.

Rename `.env.example` to `.env`.

```bash
mv .env.example .env
```

In order to enable basic authentication to access the MLflow server, NGINX is used. To create username-password pairs, [Configuring http basic authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/) guide from NGINX documentation can be followed. The path where the user's information (username:encrypted-password) is stored is the same as the one used in the guide `/etc/apache2/.htpasswd`.

When using authentication from MLflow to log experiments or models, username and password (unencrypted) need to be setted using the following environment variables. 

> **_NOTE_**: Both the user and password must be the same as those created previously to use NGINX authentication.

```python
import os
...
os.environ['MLFLOW_TRACKING_USERNAME'] = <USERNAME>
os.environ['MLFLOW_TRACKING_PASSWORD'] = <PASSWORD_UNENCRYPTED>
```
More information about authentication can be found in the MLflow´s documentation [Logging to a Tracking Server](https://mlflow.org/docs/latest/tracking.html#logging-to-a-tracking-server).


Docker compose services can be executed with the following command.
```bash
docker-compose up -d
```

Finally, you can access and login to the MLflow server through [http://127.0.0.1](http://127.0.0.1).