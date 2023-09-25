# Docker State Exporter

Exporter for docker container state

Prometheus exporter for docker container state, written in Go.

One of the best known exporters of docker container information is [cAdvisor](https://github.com/google/cadvisor).\
However, cAdvisor does not export the state of the container.

This exporter will only export the container status and the restarts count.

## Installation and Usage

The `docker_state_exporter` listens on HTTP port 8080 by default.

### Ansible

Simple Ansible Role is available in `ansible/roles/` folder.

It configures listening port on 4343 by default

## Metrics

This exporter will export the following metrics.

- `container_state_health_status`
- `container_state_status`
- `container_state_oomkilled`
- `container_state_startedat`
- `container_state_finishedat`
- `container_restartcount`

These metrics will be the same as the results of docker inspect.

This exporter also exports the standard
[Go Collector](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus#NewGoCollector)
and [Process Collector](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus#NewProcessCollector).

## Caution

This exporter will do a docker inspect every time prometheus pulls.\
If a large number of requests are made, there will be performance issues. (I think. Not verified.)\
So, this app caches the result of docker inspect for 1 second.
So, please note that if you set the scrape_interval of prometheus to less than one second, you may get the same result back.

## Development building and running

### Build

```bash
git clone https://github.com/Poil/docker_state_exporter
cd docker_state_exporter
go build
```

### Run

```bash
docker_state_exporter \
  -listen-address=:8080
```
