# Release Notes

## v1.0.0 - 2026-06-12

### Implemented Components

- Grafana `11.5.2`
- Prometheus `3.5.0`
- Alertmanager `0.28.1`
- Loki `3.4.2`
- Promtail `3.4.2`
- Tempo `2.7.2`
- Node Exporter `1.9.1`
- cAdvisor `0.49.2`

### Hardening Improvements

- Pinned all container image versions.
- Added `no-new-privileges:true` across services.
- Dropped Linux capabilities where compatible.
- Enabled read-only root filesystems where compatible.
- Mounted application config files as read-only.
- Replaced bind-mounted runtime data with named volumes.
- Added `.gitignore` entries for `.env`, real auth files, local secrets, logs, and runtime data.

### Prometheus Basic Auth

- Added `config/prometheus/web.yml.example`.
- Mounted real local `config/prometheus/web.yml` at runtime.
- Removed committed plaintext credentials.
- Configured Prometheus self-scrape through `password_file`.

### Prometheus Lifecycle Reload API

- Enabled `--web.enable-lifecycle`.
- Did not enable `--web.enable-admin-api`.

### Alertmanager Integration

- Added Prometheus `alerting` configuration for `alertmanager:9093`.
- Added minimal valid Alertmanager routing with `default-receiver`.

### Alert Rules

Added starter rules:

- `ExporterDown`
- `HighHostCPUUsage`
- `HighHostMemoryUsage`
- `HighHostDiskUsage`
- `ContainerDown`

### Grafana Provisioning

- Provisioned Prometheus, Loki, Tempo, and Alertmanager data sources.
- Configured Prometheus datasource Basic Auth through environment variable expansion.
- Added dashboard provider for the `dashboards/` directory.
- Added starter dashboards for Node Exporter and Docker container monitoring.

### Persistent Volumes

- `grafana_data`
- `prometheus_data`
- `alertmanager_data`
- `loki_data`
- `promtail_positions`
- `tempo_data`

### Validation and Tests Performed

- Compose syntax validation with `docker compose config`.
- Stack startup with `docker compose up -d`.
- Container state verification with `docker compose ps`.
- Service log checks with `docker compose logs`.
- HTTP health endpoints for Grafana, Prometheus, Alertmanager, Loki, Tempo, Node Exporter, and cAdvisor.
- Prometheus targets API.
- Prometheus rules API.
- Prometheus reload API.

### Fixed Issues

- Removed plaintext Alertmanager credentials from Prometheus configuration.
- Removed hardcoded Grafana admin password from `grafana.ini`.
- Removed hardcoded Basic Auth password from container health checks.
- Consolidated duplicate Loki, Promtail, and Tempo configuration files under `config/`.
- Avoided committing real `web.yml` files and local secret files.
- Added optional local port overrides for Node Exporter and cAdvisor while keeping portfolio defaults at `9100` and `8080`.
- Added a Promtail positions volume so Docker log offsets survive container recreation.
- Adjusted Promtail Docker log parsing to drop stale Docker log entries before sending them to Loki during first startup.
- Replaced Promtail `wget` healthcheck with a process check because the Promtail image does not ship an HTTP client.

### Known Limitations

- Single-node local lab only.
- No TLS on local endpoints.
- Alertmanager uses a default local receiver without external notifications.
- Loki and Tempo use filesystem storage suitable for lab use, not production scale.

### Next Upgrade Steps

- Add CI validation for Compose, Prometheus rules, and Grafana dashboard JSON.
- Add TLS through a reverse proxy.
- Add a demo application emitting OTLP traces and structured logs.
- Add Alertmanager notification receivers.
- Add curated screenshots for the GitHub portfolio page.
