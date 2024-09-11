# MariaDB on docker

`docker pull mariadb:latest`

`docker run --name mariadb-test -e MYSQL_ROOT_PASSWORD=0932 -v /home/passion/mariadb-data:/var/lib/mysql -p 3306:3306 mariadb:latest`

