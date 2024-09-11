# Redis

1. `[<https://redis.io/download/>](<https://redis.io/download/>)`
2. `tar -zxvf ./redis-5.0.14.tar.gz`
3. `mv ./redis-5.0.14 /usr/local/redis`
4. `cd redis-5.0.14`
5. `make`
6. `make install`
7. `./utils/install_server.sh`
8. `systemctl status redis_6379.service`
9. `systemctl restart redis_6379.service`
10. `redis-cli` // 自带的客户端
11. `vim /etc/redis/6379.conf`

`# bind 127.0.0.1` // 允许远程访问 `requirepass password` // 设置密码

12. `systemctl restart redis_6379.service`
13. `firewall-cmd --zone=public --add-port=6379/tcp --permanent` // 开放端口
14. `firewall-cmd --reload`

