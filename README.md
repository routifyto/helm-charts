# Routify Helm Chart

## Introduction

This Helm chart deploys the Routify project on a Kubernetes cluster. Routify is an open-source AI gateway that routes LLM requests to various providers with no code changes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure (if persistence is required)

## Components

This chart will deploy the following components of the Routify project:

1. API
2. Gateway
3. Admin
4. Migrations (as a Kubernetes Job)
5. PostgreSQL (optional)
6. Redis (optional)

## Installing the Chart

To install the chart with the release name `my-routify`:

```bash
helm install my-routify ./routify-0.1.0.tgz
```

This command deploys Routify on the Kubernetes cluster with default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-routify` deployment:

```bash
helm uninstall my-routify
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Routify chart and their default values.

| Parameter                   | Description                                     | Default                     |
|-----------------------------|-------------------------------------------------|-----------------------------|
| `global.environment`        | Global environment setting                      | `production`                |
| `postgresql.enabled`        | Deploy PostgreSQL                               | `true`                      |
| `postgresql.auth.username`  | PostgreSQL username                             | `routify_user`              |
| `postgresql.auth.password`  | PostgreSQL password                             | `routify_password`          |
| `postgresql.auth.database`  | PostgreSQL database name                        | `routify_ai`                |
| `redis.enabled`             | Deploy Redis                                    | `true`                      |
| `api.replicaCount`          | Number of API replicas                          | `1`                         |
| `api.image.repository`      | API image repository                            | `your-docker-registry/routify-api` |
| `api.image.tag`             | API image tag                                   | `latest`                    |
| `gateway.replicaCount`      | Number of Gateway replicas                      | `1`                         |
| `gateway.image.repository`  | Gateway image repository                        | `your-docker-registry/routify-gateway` |
| `gateway.image.tag`         | Gateway image tag                               | `latest`                    |
| `frontend.replicaCount`     | Number of Frontend replicas                     | `1`                         |
| `frontend.image.repository` | Frontend image repository                       | `your-docker-registry/routify-frontend` |
| `frontend.image.tag`        | Frontend image tag                              | `latest`                    |
| `migrations.image.repository` | Migrations image repository                   | `your-docker-registry/routify-migrations` |
| `migrations.image.tag`      | Migrations image tag                            | `latest`                    |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```bash
helm install my-routify ./routify-0.1.0.tgz \
  --set postgresql.enabled=false \
  --set externalDatabase.host=my-external-postgresql
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-routify ./routify-0.1.0.tgz -f my-values.yaml
```

## Configuration and installation details

### External Database / Redis Support

To use an external database or Redis instance, set `postgresql.enabled` or `redis.enabled` to `false` and configure the `externalDatabase` or `externalRedis` parameters.

Example:

```yaml
postgresql:
  enabled: false

externalDatabase:
  host: my-external-postgresql
  port: 5432
  user: my-user
  password: my-password
  database: my-database

redis:
  enabled: false

externalRedis:
  host: my-external-redis
  port: 6379
```

### Migrations

The migrations are run as a Kubernetes Job. The job is set to run before the API service starts, ensuring that the database schema is up to date.

### Ingress

This chart does not include an Ingress resource by default. To expose the services externally, you may need to configure an Ingress based on your cluster setup.

## Upgrading

### To 1.0.0

This is the first release of the Routify Helm chart. There are no special upgrade considerations at this time.

## License

This Helm chart is open-source and available under the [MIT License](LICENSE).

## Contributing

We welcome contributions to this Helm chart. Please see our [Contributing Guide](CONTRIBUTING.md) for more details.