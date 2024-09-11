# Install Grafana

## Reference

[Install Grafana |  Grafana documentation](https://grafana.com/docs/grafana/latest/setup-grafana/installation/)

## MacOS Brew

`brew install grafana`

### Install Plugins

**Clickhouse**

[ClickHouse plugin for Grafana | Grafana Labs](https://grafana.com/grafana/plugins/grafana-clickhouse-datasource/?tab=installation)

`grafana cli plugins install grafana-clickhouse-datasource`

**Install Manually**

### Brew

`cp -r /usr/local/var/lib/grafana/plugins/* /opt/homebrew/var/lib/grafana/plugins`

### Linux

`/var/lib/grafana/plugins`

## Docker

[Run Grafana Docker image |  Grafana documentation](https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/)

