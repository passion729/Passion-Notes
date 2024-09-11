# Install Docker on Ununtu Server

## Use aliyun script

`curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun`

## Change root privilege

<procedure>
<step><code>sudo groupadd docker</code></step>
<step><code>sudo gpasswd -a ${USER} docker</code></step>
<step><code>newgrp docker</code></step>
</procedure>

To run Docker as a non-privileged user, consider setting up the

Docker daemon in rootless mode for your user:

[dockerd-rootless-setuptool.sh](http://dockerd-rootless-setuptool.sh) install

Visit [https://docs.docker.com/go/rootless/](https://docs.docker.com/go/rootless/) to learn about rootless mode.

To run the Docker daemon as a fully privileged service, but granting non-root

users access, refer to [https://docs.docker.com/go/daemon-access/](https://docs.docker.com/go/daemon-access/)

## Install docker-compose

`sudo apt install docker-compose`

## Change mirrors

1. `sudo vim /etc/docker/daemon.json`
2. Add

   `{"registry-mirrors": ["http://hub-mirror.c.163.com","https://registry.docker-cn.com","https://docker.mirrors.ustc.edu.cn"]}`

1. `sudo service docker restart`

## Install Portainer

1. `docker volume create portainer_data`
2. `docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest`
3. `https://xxx.xxx.xxx.xxx:9443`

## Enable Daemon Tcp connection

[Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/)

> **The difference in Ubuntu22.04**

> `sudo vim /lib/systemd/system/docker.service`

> Find and modify:

```shell
[Service]
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```

> `sudo systemctl daemon-reload`

> `sudo systemctl restart docker`

> Or:

[](https://linuxhandbook.com/docker-remote-access/)

> **Why use daemon.json doesn’t work?**

> I don’t know!

Engine Api Url: tcp://xxx.xxx.xxx.xxx:xx

## Set Proxy

[配置docker pull代理_docker pull 代理-CSDN博客](https://blog.csdn.net/vic_qxz/article/details/130061661)

### Manage docker context

[Docker contexts](https://docs.docker.com/engine/context/working-with-contexts/)

## Some post steps for reference

[](https://docs.docker.com/engine/install/linux-postinstall/)

