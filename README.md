# Prometheus

Set up common prometheus configurations

## Install to requirements.yml

```
- src: git+git@github.com:smartlogic/ansible-role-prometheus
  name: prometheus
  version: 0.1.0
```

## Requirements

None

## Role Variables

- `prometheus_version` - Which version of prometheus to download
- `prometheus_checksum` - The checksum for the version of prometheus
- `node_exporter_version` - Which version of node exporter to download
- `node_exporter_checksum` - The checksum for the version of node exporter
- `alertmanager_version` - Which version of alertmanager to download
- `alertmanager_checksum` - The checksum for the version of alertmanager
- `redis_exporter_version` - Which version of redis_exporter to download
- `redis_exporter_checksum` - The checksum for the version of redis_exporter
- `grafana_ini_file` - The file to use for `grafana.ini`
  - Default: `grafana.ini`
- `prometheus_config_file` - The file to use for `prometheus.yml`
  - Default: `prometheus.yml`
- `prometheus_alert_file` - The file to use for `alertmanager.yml`
  - Default: `alertmanager.yml`
- `alertmanager_enabled` - boolean - True for enabling the alertmanager, via systemctl
  - Default: `true`
- `prometheus` - Configure scrape files, alert rules, and alert templates

## Dependencies

None

## Example Configuration

```yaml
grafana_ini_file: "{{ playbook_dir }}/files/grafana.ini"
prometheus_config_file: "{{ playbook_dir }}/files/prometheus.yml"
prometheus_alert_file: "{{ playbook_dir }}/files/alertmanager.yml"

prometheus:
  jobs:
    - "{{ playbook_dir }}/file_config/project.json"
  alert:
    rules:
      - "{{ playbook_dir }}/alert_rules/rules.yml"
    templates:
      - "{{ playbook_dir }}/alert_templates/templte.tmpl"
```

## Example Playbook

Full setup:

```yaml
- hosts: servers
  roles:
    - { role: prometheus, action: "full" }
```

Node exporter only:

```yaml
- hosts: servers
  roles:
    - { role: prometheus, action: "node_exporter" }
```

Redis exporter:

```yaml
- hosts: servers
  roles:
    - { role: prometheus, action: "redis_exporter" }
```

## License

MIT

## Author Information

SmartLogic. https://smartlogic.io
