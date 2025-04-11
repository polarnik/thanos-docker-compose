# Thanos Docker Compose

Run a basic Thanos setup for local development using Docker and Docker Compose.

## Prerequisites

- You need to have Docker and docker-compose installed on your machine.

## Usage

### Start the dev environment

- Run `docker compose up`

### Stop the dev environment

- Run `docker compose down`

### Checking status of containers

- You can run `docker-compose ps` to list all containers running.
- Run `docker-compose logs <container-name>` to view logs for a container.

Refer to `docker-compose` documentation for a full overview.

### Connecting to minio

After running `docker compose up` you would be able to access `minio` at `http://localhost:40259`. The access key is `myaccesskey` and the secret key is `mysecretkey`.

## What does it start?

| Service               |                                                                              | Ports |
| ------------------    |------------------------------------------------------------------------------|-------|
| prometheus_one        | The first Prometheus server                                                  | 9001  |
| prometheus_two        | The second Prometheus server                                                 |       |
| minio                 | A minio instance serving as Object Storage for store, compactor and sidecars | 9000  |
| minio                 | A minio WebUI                                                                | 8999  |
| thanos_sidecar_one    | First Thanos sidecar for prometheus_one                                      |       |
| thanos_sidecar_two    | Second Thanos sidecar for prometheus_two                                     |       |
| thanos_querier        | Thanos querier instance connected to both sidecars and Thanos store          | 10902 |
| thanos_query_frontend | Thanos query frontend connected to querier                                   | 19090 |
| thanos_compactor      | Thanos compactor running with `--wait` and `--wait-interval=3m`              | 10922 |
| thanos_store          | A Thanos store instance connected to minio                                   | 10912 |

### Optional services

There are some services which are commented out in the `docker-compose.yml`. You can uncomment and use them if needed.

| Service      |                                                                                                          | Ports |
| ------------ | -------------------------------------------------------------------------------------------------------- | ----- |
| grafana      | A Grafana instance with username = admin, and password = admin                                           | 3000  |
| bucket_web   | Thanos Bucket Inspector WebUI                                                                            | 8080  |
| debug        | A debug container running on ubuntu with `thanos` binary. You can use this to debug services from inside | 10902 |
| alertmanager | Prometheus Alertmanager to send alerts                                                                   | 9093  |
| thanos_rule  | Thanos Rule instance to create recording and alerting rules                                              | 10932 |

You can start a debug container and/or Thanos Bucket WebUI by uncommenting the corresponding definition in `docker-compose.yml`.

For Alertmanager to work, you need to set the Slack webhook URL in `prometheus/alertmanager.yml`.

## Credits

The `docker-compose.yml` is based on [PR#2583](https://github.com/thanos-io/thanos/pull/2583) on Thanos GitHub repo by [Darshan Chaudhary](https://github.com/darshanime)
