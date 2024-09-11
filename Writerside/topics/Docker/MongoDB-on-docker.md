# MongoDB on docker

`docker pull mongo:latest`

`docker run -it --name mongodb-test -p 27017:27017 -v /xxx/mongo-data:/data/db -d mongodb:latest`

`docker exec -it mongodb-test mongosh`

`use admin`

`db.createUser({user:”username”,pwd:”passwd”,roles:[“root”]})`

`db.auth(“username”, “passwd”)`

`mongosh --quiet mongodb://username:’passwd’@127.0.0.1:27017`

