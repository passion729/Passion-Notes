# Java

1. `rpm -qa | grep java` // 查找已安装jdk
2. `yum remove -y javaxxx` //卸载已安装jdk
3. `[https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/)`
4. `tar -zxvf ./jdkxxx`
5. `mv ./jdkxxx /usr/local/jdk`
6. `vim /etc/profile`
7. `export JAVA_HOME=/usr/local/jdk`
8. `export CLASSPATH=$JAVA_HOME/lib`
9. `export PATH=$PATH:$JAVA_HOME/bin`
10. `source /etc/profile`
11. `java -version`
12. `javac`

