# Prometheus

Set up common prometheus configurations

## Install to requirements.yml

```
- src: git+git@github.com:smartlogic/ansible-role-prometheus
  name: prometheus
  version: 0.1.0
```

## Requirements

### For prometheus-postgresql-adapter

* PostgreSQL 10
* A PostgreSQL user that has full permissions on the database prometheus will be using
* This assumes a local connection

If enabled, this will set up two PostgreSQL extensions: [pg_prometheus][pg_prometheus] and [timescaledb][timescaledb]. [prometheus-postgresql-adapter][prometheus-postgresql-adapter] will also be set up as a service.

## Role Variables

- `prometheus_version` - Which version of prometheus to download
- `prometheus_checksum` - The checksum for the version of prometheus
- `alertmanager_version` - Which version of alertmanager to download
- `alertmanager_checksum` - The checksum for the version of alertmanager
- `grafana_ini_file` - The file to use for `grafana.ini`
  - Default: `grafana.ini`
- `prometheus_config_file` - The file to use for `prometheus.yml`
  - Default: `prometheus.yml`
- `prometheus_alert_file` - The file to use for `alertmanager.yml`
  - Default: `alertmanager.yml`
- `alertmanager_enabled` - boolean - True for enabling the alertmanager, via systemctl
  - Default: `true`
- `prometheus` - Configure scrape files, alert rules, and alert templates
- `prometheus_postgresql_archive` - boolean - If true the postgresql adapter will be installed
  - Default: `false`
- `prometheus_postgresql_database` - Database that the adapter will use
  - Default: `metrics`
- `prometheus_postgresql_username` - User that the adapter will connect as
  - Default: `prometheus`

## Dependencies

None

## Example Configuration

```yaml
grafana_ini_file: "{{ playbook_dir }}/files/grafana.ini"
prometheus_config_file: "{{ playbook_dir }}/files/prometheus.yml"
prometheus_alert_file: "{{ playbook_dir }}/files/alertmanager.yml"

prometheus_postgresql_archive: true
prometheus_postgresql_database: metrics
prometheus_postgresql_username: prometheus

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

## License

MIT

## Author Information

SmartLogic. https://smartlogic.io

[prometheus-postgresql-adapter]: https://github.com/timescale/prometheus-postgresql-adapter
[pg_prometheus]: https://github.com/timescale/pg_prometheus
[timescaledb]: https://github.com/timescale/timescaledb
