# MySql

1. `rpm -qa|grep mariadb` // 卸载Mariadb
2. `yum -y remove mariadbxxx`
3. `tar -zxvf ./mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz`
4. `mv ././mysql-5.7.30-linux-glibc2.12-x86_64 /usr/local/mysql`
5. `groupadd mysql`
6. `useradd -g mysql mysql`
7. `mkdir -p /usr/local/mysql/data`
8. `chown -R mysql:mysql /usr/local/mysql`
9. `vim /etc/my.cnf`

```Bash
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock

[mysqld]
skip-name-resolve
# 设置3306端⼝
port = 3306
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装⽬录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放⽬录
datadir=/usr/local/mysql/data
# 允许最⼤连接数 
max_connections=200
# 服务端使⽤的字符集默认为8⽐特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使⽤的默认存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
```

   1. `mkdir /var/lib/mysql`
   2. `chmod 777 /var/lib/mysql`
   3. `cd /usr/local/mysql`
   4. `./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data` // 记住输出的密码
   5. `cp ./support-files/mysql.server /etc/init.d/mysqld`
   6. `vim /etc/init.d/mysqld`

   `basedir=/usr/local/mysql`

   `datadir=/usr/local/mysql/data`

   1. `chmod +x /etc/init.d/mysqld`
   2. `chkconfig --add mysqld`
   3. `chkconfig --list mysqld` // 2345均为on
   4. `service mysqld start`
   5. `vim /etc/profile`

   `export PATH=$PATH:/usr/local/mysql/bin`

   1. `source /etc/profile`
   2. `yum install ncurses-compat-libs` // 如果报连接库错误
   3. `alter user user() identified by "111111";` // 设置数据库密码
   4. `flush privileges;`
   5. `use mysql;`
   6. `update user set user.Host='%' where user.User='root';` // 设置远程登录
   7. `flush privileges;`
   8. `firewall-cmd --zone=public --add-port=8000/tcp --permanent && firewall-cmd --reload`  // 开放3306端口

