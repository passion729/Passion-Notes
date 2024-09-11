# Nginx

1. [Nginx](http://nginx.org/en/download.html)
2. `tar -zxvf ./nginx-1.20.2.tar.gz`
3. `cd ./nginx-1.20.2`
4. `./congigure --prefix=/usr/local/nginx`
5. `sudo apt install -y make libpcer3 libpcre3-dev openssl openssl-devel`
6. `make`
7. `make install`
8. `/usr/local/nginx/sbin/nginx -v`

**启动Nginx**

`/usr/local/nginx/sbin/nginx`

**停止Nginx**

`/usr/local/nginx/sbin/nginx -s stop`

**重新加载Nginx**

`/usr/local/nginx/sbin/nginx -s reload`

**配置文件**

`/usr/local/nginx/conf/nginx.conf`

