# Maven

1. `[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)`
2. `tar -zxf ./apache-maven-3.6.2-bin.tar.gz`
3. `mv ./apache-maven-3.6.2-bin /usr/local/maven`
4. `vim /etc/profile`
5. `export MVN_HOME=/usr/local/maven`
6. `export PATH=$PATH:$MVN_HOME/bin`
7. `source /etc/profile`
8. `mvn -version`

