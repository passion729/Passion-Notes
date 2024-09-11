# MySql on Docker

## **Install Docker Image**

1. `docker pull mysql/mysql-server`
2. `docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server`
3. `docker exec -it mysql bash` // Get into shell

## **Reset SQL Password**

1. `mysql -u root -p`
2. `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`
3. `FLUSH PRIVILEGES;`

## **Remote User Login**

1. `mysql -u root -p`
2. `create user 'root'@'%' identified by 'password';`
3. `GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;`
4. `FLUSH PRIVILEGES;`

