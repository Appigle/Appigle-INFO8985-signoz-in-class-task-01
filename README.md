# signoz
signoz demo with docker-compose 

```bash
ansible-playbook up.yml
```

This does docker compose up on an "empty" signoz in a codespace. Use this as a starting point for instrumenting your app.

```bash
ansible-playbook down.yml
```

This does docker compose down on the clickhouse-setup/docker-compose-minimal.yaml (the same docker-compose file from up.yml)

---

## Issue fix explanation

```
   - image: signoz/frontend:${DOCKER_TAG:-0.56.0}
   + image: signoz/frontend:${DOCKER_TAG:-0.73.0}
```
1. Change the signoz version (`clickhouse-setup/docker-compose-minimal.yaml`) from 0.56.0 to 0.73.0 for new feature which we used as this feature doesn't exist in v0.56.0.

```
      - type: filter
        id: signoz_logs_filter
        expr: 'attributes.container_name matches "^signoz-(logspout|frontend|alertmanager|query-service|otel-collector|clickhouse|zookeeper)"'
      # - type: filter
      #   id: signoz_logs_filter
      #   expr: 'attributes.container_name matches "^signoz-(logspout|frontend|alertmanager|query-service|otel-collector|clickhouse|zookeeper)"'
```
2. Remove the log filter from `clickhouse-setup/otel-collector-config.yaml` to ensure the program running correctly.

## How to get the metric?

```yaml
  hostmetrics:
    collection_interval: 30s
    root_path: /hostfs
    scrapers:
      cpu: {}
      load: {}
      memory: {}
      disk: {}
      filesystem: {}
      network: {}
```
In config file: `/clickhouse-setup/otel-collector-config.yaml`, we can set the collection metrics data
