# Clickhouse on Docker

```bash
docker run --rm -d --name temp-clickhouse clickhouse/clickhouse-server
docker cp temp-clickhouse:/etc/clickhouse-server/users.xml /xxx/users.xml
docker cp temp-clickhouse:/etc/clickhouse-server/config.xml /xxx/config.xml
```

```bash
# config.xml
<listen_host>::1</listen_host>
<listen_host>127.0.0.1</listen_host>

# users.xml
<password>123456</password>
```

```bash
docker run \
-d --name=clickhouse-server \
-p 8123:8123 \
-p 9000:9000 \
-v $DOCKERDATA/clickhouse-test/config.xml:/etc/clickhouse-server/config.xml \
-v $DOCKERDATA/clickhouse-test/users.xml:/etc/clickhouse-server/users.xml \
-v $DOCKERDATA/clickhouse-test/data:/var/lib/clickhouse \
clickhouse/clickhouse-server
```

