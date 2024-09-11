# Netdata Multi nodes

[](https://learn.netdata.cloud/docs/agent/streaming)

[](https://arc.net/l/quote/rifgpkyq)

## Slave Node

`sudo vim /etc/netdata/netdata.conf`

```NGINX
[global]
    memory mode = none # 在此主机上禁用数据库。这也会禁用运行状况监视（没有数据库就无法进行运行状况监视）
    hostname = chenshaolang-MS-7817 # 修改配置名称
[web]
    mode = none  # 禁用API（Netdata不会监听任何端口）。这也会禁用注册表（没有API的注册表将无法使用)
```

`sudo vim /etc/netdata/stream.conf`

```bash
[stream]
  enabled = yes
  destination = 127.0.0.1:19999 # 主服务器的ip地址
  api key = 733d1bb7-91b1-4bcf-9a45-66ec2340316e   
  # api key是一个uuid格式的字符串，可以使用uuidgen命令生成。主要是提供给主服务器使用
```

`sudo systemctl restart netdata`

`uuidgen`

## Master Node

`sudo vim /etc/netdata/stream.conf`

```bash
[733d1bb7-91b1-4bcf-9a45-66ec2340316e]  # 刚才节点服务器生成的api key
    enabled = yes
    default history = 3600
    default memory mode = save
    health enabled by default = auto 
    allow from = *  # allow from可以设置数据流的允许来源以保证安全。如 allow from = 127.0.0.1 仅允许该ip连接
```

`sudo systemctl restart netdata`

