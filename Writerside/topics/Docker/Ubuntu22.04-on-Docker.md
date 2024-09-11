# Ubuntu22.04 on Docker

- `docker pull ubuntu`
- `docker run -it --name ubuntu-test ubuntu bash`
- `cat /etc/issue` // System version
- `passwd` // Set root password
- // 选装`apt-get install lsb-core`
   - `lsb_release -a | grep Codename | awk '{print $2}'` // 输出Codename
- // 换源
- `apt install vim`
- `cd /etc/apt`
- `mv sources.list sources.list.bak`
- `vi sources.list`
- //arm 镜像链接需加上-ports
- // Docker 不支持https，需改成http

```shell
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy main restricted universe multiver
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
```

- `apt update`
- `apt install openssh-server`
- `vim /etc/ssh/sshd_config`
   - uncomment`l34: PermitRootLogin prohibit-password`
   - `l39:PubkeyAuthentication yes`
   - `l42:` `AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2`
- `/etc/init.d/ssh restart`
- `mkdir ~/.ssh`
- `touch ~/.ssh/authorized_keys`
- 在本机mac终端，cat ~/.ssh/id_rsa.pub  如果没有该文件，终端输入ssh-keygen，连续回车enter，即生成该文件, 将本机id_rsa.pub的一行内容，vim复制到docker容器的 ~/.ssh/authorized_keys中
- `exit`
- `docker commit -m 'add ssh' -a 'author-name' <container id> ubuntu-ssh`
- `docker rm ubuntu-test`
- `docker run -d -p 2233:22 --name ubuntu ubuntu-ssh /usr/sbin/sshd -D`
- `ssh -p 2233 root@localhost`
- 在本机 macOS，vim ~/.ssh/config，添加如下内容：

   `Host ubuntuHostName localhostUser     rootPort     2233`

   然后可以 ssh ubuntuTest 连接容器

