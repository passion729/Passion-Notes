# Redis on docker

[redis.conf](https://github.com/redis/redis/blob/7.2/redis.conf)

```bash
# bind 127.0.0.1 -::1
bind 0.0.0.0

reqiurepass foobared
```

```bash
redis-data
├── data
└── redis.conf
```

```bash
docker pull redis:latest

docker run --restart=unless-stopped \
-p 6379:6379 \
--name redis-test \
-v $DATA/redis-data/redis.conf:/etc/redis/redis.conf \
-v $DATA/redis-data/data:/data \
-d redis:latest redis-server /etc/redis/redis.conf
```

