# Django Apllication Chart

## Introduction

This chart deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

It also packages the [PostgreSQL](https://github.com/kubernetes/charts/tree/master/stable/postgresql)

## Install Kubernetes, Helm, Kubeapps

- [Install Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Install Helm](https://docs.helm.sh/using_helm/#installing-helm)
- [Install Kubeapps](https://github.com/kubeapps/kubeapps/blob/master/docs/getting-started.md#installation-of-the-kubeapps-installer)

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- helm >= v2.3.0 to run "pre-install" hooks in right order.

## Installing the Chart

To install the chart:

```
$ helm install --name my-release django-application
```

## Configuration

### The Api configuration.

| Parameter                                   | Description                                                        | Default                                           |
| ------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------- |
| `api.name`                                  | Api image name                                                     | `api-deployment`                                  |
| `api.image`                                 | Api image                                                          | `gcr.io/samples-192203/sample-employees`          |
| `api.imageTag`                              | Api imag version                                                   | `v4`                                              |
| `api.replicas`                              | Number of replicas                                                 | `1`                                               |
| `api.containerPort`                         | Number port to access                                              | `8000`                                            |
| `api.healthzPath`                           | Path to check the image container healthy                          | `/`                                               |
| `api.initialDelaySeconds`                   | Deplay time to initial, by second                                  | `10`                                              |
| `api.periodSeconds`                         | Period time, by second                                             | `30`                                              |
| `api.restartPolicy`                         | Restart policy                                                     | `Always`                                          |
| `api.imagePullPolicy`                       | Image pull policy                                                  | `Always`                                          |
| `api.type`                                  | ClusterIP, NodePort, or LoadBalancer                               | `NodePort`                                        |
| `api.port`                                  | Api port to use                                                    | `8000`                                            |
| `api.targetPort`                            | Api target port to use                                             | `8000`                                            |
| `api.cloudsqlOauthCredentials.name`         | Volume cloudsql-oauth-credentials                                  | `cloudsql-oauth-credentials`                      |
| `api.cloudsqlOauthCredentials.mountPath`    | Path mount to folder                                               | `/secrets/cloudsql`                               |
| `api.cloudsql.name`                         | Volume Clousql                                                     | `cloudsql`                                        |

### The Database configuration.

| Parameter                                   | Description                                                        | Default                                           |
| ------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------- |
| `database.restartPolicy`                    | Restart policy                                                     | `Never`                                           |
| `database.name`                             | Database name                                                      | `gcloud-sqlproxy`                                 |
| `database.image`                            | Database image                                                     | `gcr.io/cloudsql-docker/gce-proxy`                |
| `database.imageTag`                         | Database image version                                             | `1.11`                                            |
| `database.serviceAccountKey`                | Service account key                                                | `Must be provided and base64 encoded`             |
| `database.cloudsql.instance`                | PostgreSQL/MySQL instance name                                     | `samples-192203:us-central1:samples-employees`    |
| `database.cloudsql.port`                    | PostgreSQL/MySQL instance port                                     | `5432`                                            |
| `database.cloudsql.credential_file`         | Path to credential file                                            | `/secrets/cloudsql/credentials.json`              |
| `database.volumeMounts.name`                | Volume name                                                        | `cloudsql-oauth-credentials`                      |
| `database.volumeMounts.mountPath`           | Path mount to folder                                               | `/secrets/cloudsql`                               |
| `database.volumeMounts.readOnly`            | Only read folder                                                   | `true`                                            |

### The Cheese configuration.

| Parameter                                   | Description                                                        | Default                                           |
| ------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------- |
| `cheese.restartPolicy`                      | Restart policy                                                     | `Always`                                          |
| `cheese.imagePullPolicy`                    | Image pull policy                                                  | `Always`                                          |
| `cheese.replicas`                           | Number of replicas                                                 | `1`                                               |
| `cheese.name`                               | Image name                                                         | `cheese`                                          |
| `cheese.image`                              | Image repo                                                         | `gcr.io/samples-192203/cheese`                    |
| `cheese.containerPort`                      | Container port                                                     | `80`                                              |
| `cheese.cheddar`                            | Cheese image version                                               | `cheddar`                                         |
| `cheese.stilton`                            | Cheese image version                                               | `stilton`                                         |
| `cheese.type`                               | ClusterIP, NodePort, or LoadBalancer                               | `NodePort`                                        |
| `cheese.port`                               | Port to use                                                        | `8000`                                            |
| `cheese.targetPort`                         | Target port to use                                                 | `8000`                                            |

### The Ingress configuration

| Parameter                                   | Description                                   | Default                               |
| ------------------------------------------- | --------------------------------------------- | ------------------------------------- |
| `ingress.enabled`                           | Condition to enable or not create Ingress     | `true`                                |
| `ingress.host`                              | Host name                                     | `mydjango`                            |
| `ingress.name`                              | Ingress's name                                | `api-ingress`                         |
| `ingress.annotations`                       | Annotations                                   |                                       |
| `ingress.tls`                               | Transport Layer Security                      |                                       |

### The Secrets configuration.

| Parameter                                       | Description                       | Default                                         |
| ----------------------------------------------- | --------------------------------- | ----------------------------------------------- |
| `secret.cloudsqlOauthCredentials.name`          | Name of secret                    | `cloudsql-oauth-credentials`                    |
| `secret.cloudsqlOauthCredentials.data`          | Service account secret key        | `Must be provided and base64 encoded`           |
| `secret.cloudsqlDbCredentials.name`             | Name of secret                    | `backend-secrets`                               |
| `secret.cloudsqlDbCredentials.username`         | Username to login database        | `admin`                                         |
| `secret.cloudsqlDbCredentials.password`         | Password to login database        | `admin123`                                      |
| `secret.backendSecrets.name`                    | Name of secret                    | `backend-secrets`                               |
| `secret.backendSecrets.host`                    | Host database                     | `127.0.0.1`                                     |
| `secret.backendSecrets.port`                    | Port database                     | `5432`                                          |
| `secret.default.username`                       | Username default                  | `postgres`                                      |
| `secret.default.password`                       | Password default                  | `postgres`                                      |

## Basic commands
Get list helm installed
```
$ helm list
```

Remove helm installed
```
$ helm delete my-release
```

Read more command line at here: https://docs.helm.sh/helm/#helm
