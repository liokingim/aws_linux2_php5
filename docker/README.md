ping -c 3 8.8.8.8


docker pull mysql:5.7.44

docker pull httpd:2.4.58

docker pull php:5.6.40

docker pull redis

docker pull wordpress:php5.6

docker pull wordpress:php5.6-apache

docker pull jboss/wildfly:10.1.0.Final

docker pull bitnami/wildfly:10.1.0-r35

docker pull java:openjdk-8u111

docker pull openjdk:8u342-jdk

docker container exec -ti app-web-1 bash

docker compose up

docker compose down

2.Vimのインストール

OSによってインストール方法が変わる。

・Redhat系

yum install vim

・Debian/Ubuntu系

apt-get update
apt-get install vim

https://zenn.dev/junki555/articles/1073408293050f


grant all privileges on *.* to "root"@"%" identified by "123456";
 flush privileges;