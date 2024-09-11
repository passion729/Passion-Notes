# Install Clickhouse on Ubuntu

[https://clickhouse.com/docs/en/install](https://clickhouse.com/docs/en/install)

### Set up the repository

```shell
sudo apt-get install -y apt-transport-https ca-certificates dirmngr
GNUPGHOME=$(mktemp -d)
sudo GNUPGHOME="$GNUPGHOME" gpg --no-default-keyring --keyring /usr/share/keyrings/clickhouse-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8919F6BD2B48D754
sudo rm -rf "$GNUPGHOME"
sudo chmod +r /usr/share/keyrings/clickhouse-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/clickhouse-keyring.gpg] https://packages.clickhouse.com/deb stable main" | sudo tee \
    /etc/apt/sources.list.d/clickhouse.list
sudo apt-get update
```

### Install Clickhouse Server and Client

```shell
sudo apt-get install -y clickhouse-server clickhouse-client

# or use the offline package
sudo dpkg -i xxxxxxx
```

> [The offline package download link](https://packages.clickhouse.com/deb/pool/main/c/)

> `clickhouse-common-static_22.3.10.22_amd64.deb`

> `clickhouse-server_22.3.10.22_amd64.deb`

> `clickhouse-client_22.3.10.22_amd64.deb`

## Setup Remote Access

```bash
sudo vim etc/clickhouse-server/config.xml
IP4：放开<listen_host>::</listen_host>的注释
IP6：放开<listen_host>0.0.0.0</listen_host>的注释
```

### Start ClickHouse server

```shell
sudo service clickhouse-server start
clickhouse-client
# or "clickhouse-client --password" if you've set up a password.
```

