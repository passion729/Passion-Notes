# RabbitMQ on Docker

- `docker pull rabbitmq`
- `docker run --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq`
- `docker exec -it rabbitmq bash`
- `rabbitmqctl status`
- `rabbitmq-plugins enable rabbitmq_management    // enable web management`
- `rabbitmqctl add_user username password    // add user`
- `rabbitmqctl set_user_tags username administrator    // add admin`
- `cd /etc/rabbitmq/conf.d/`
- `echo management_agent.disable_metrics_collector = false > management_agent.disable_metrics_collector.conf`
- `exit`
- `docker restart rabbitmq`

