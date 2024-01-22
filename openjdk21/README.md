groupadd tms2_tomcat
useradd -s /sbin/nologin -g tms2_tomcat tms2_tomcat
id tms2_tomcat
useradd -s /sbin/nologin tomcat
id tomcat

mkdir -p /usr/local/java

tar zxvf openjdk-21.0.2_linux-x64_bin.tar.gz

mv jdk-21.0.2 /usr/local/java

vi ~/.bashrc

export JAVA_HOME=/usr/local/java/jdk-21.0.2
export PATH=$PATH:$JAVA_HOME/bin

source ~/.bashrc

java -version

tar zxvf apache-tomcat-9.0.83.tar.gz

mv apache-tomcat-9.0.83 /opt/

chown -R tms2_tomcat /opt/apache-tomcat-9.0.83

