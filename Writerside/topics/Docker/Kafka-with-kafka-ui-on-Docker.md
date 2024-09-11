# Kafka with kafka-ui on Docker

[docker-compose.yml](https://github.com/confluentinc/cp-all-in-one/blob/7.6.1-post/cp-all-in-one/docker-compose.yml)

Add:

```shell
services:
  xxxxxxxx

  kafka-ui:
    container_name: kafka-ui
    image: provectuslabs/kafka-ui:latest
    ports:
      - 18080:8080
    environment:
      DYNAMIC_CONFIG_ENABLED: 'true'
```

