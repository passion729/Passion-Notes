# Install Harbor on Ubuntu Server

[](https://goharbor.io/docs/2.9.0/install-config/)

**First need to install docker and docker-compose**

1. Download [https://github.com/goharbor/harbor/releases](https://github.com/goharbor/harbor/releases)
2. 09i909099009090909232019-3990-930-129123213313212234324
3. `tar -xzvf harbor-offline-installer-version.tgz`
4. `cd ./harbor`
5. Create `harbor.yml` configuration file or use the template

   `cp ./harbor.yml.tmpl ./harbor.yml`

1. Set the host name and DO NOT USE HTTPS
2. `sudo ./install.sh`
3. Set the **client** docker’s `daemon.json` to disable docker’s https registry:

   `{"insecure-registries" : ["myregistrydomain.com:5000", "0.0.0.0"]}`

1. The default user and password is `admin` `Harbor12345`
2. On client, use `docker login myregistrydomain.com` to use the registry, use `docker logout myregistrydomain.com` to logout
3. `docker tag nginx myregistrydomain.com/project/repository:tag`
4. `docker push .....`
5. > **First stop docker-compose, then modify the configuration file, then use `./prepare` script refresh harbor configuration, last start the docker-compose**

