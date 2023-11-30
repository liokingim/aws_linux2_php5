sudo yum update -y


ssh　インストール
sudo vi /etc/ssh/sshd_config

■sshdの再起動
$ sudo systemctl restart sshd

아파치 설치확인

httpd -v

php 설치 확인
php -v

아파치 설치
sudo yum install httpd

아파치 서비스 시작
sudo systemctl start httpd.service

아파치 프로세스 확인
ps aux |grep httpd

아파치 상태 확인
systemctl status httpd

● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-11-27 03:42:47 UTC; 2h 5min ago
     Docs: man:httpd.service(8)
 Main PID: 6978 (httpd)
   Status: "Total requests: 7; Idle/Busy workers 100/0;Requests/sec: 0.000929; Bytes served/sec:   9 B/sec"
   CGroup: /system.slice/httpd.service
           ├─6978 /usr/sbin/httpd -DFOREGROUND
           ├─6980 /usr/sbin/httpd -DFOREGROUND
           ├─6981 /usr/sbin/httpd -DFOREGROUND
           ├─6982 /usr/sbin/httpd -DFOREGROUND
           ├─6983 /usr/sbin/httpd -DFOREGROUND
           ├─6984 /usr/sbin/httpd -DFOREGROUND
           └─7026 /usr/sbin/httpd -DFOREGROUND

sudo systemctl enable httpd.service

sudo systemctl start httpd.service

sudo systemctl stop httpd.service

sudo ​systemctl restart httpd.service

php 설치에 필요한 모듈 설치
sudo yum install -y gcc make libxml2-devel openssl-devel bzip2-devel libcurl-devel libjpeg-devel libpng-devel libicu-devel libmcrypt-devel libxslt-devel httpd-devel

sudo yum -y install gcc-c++ libxml2-devel openssl-devel bzip2-devel libcurl-devel curl-devel libpng-devel libjpeg-devel freetype-devel t1lib-devel gmp-devel  libicu-devel openldap-devel libmcrypt-devel mysql-devel readline-devel net-snmp-devel libxslt-devel libXpm-devel libevent libevent-devel httpd-devel autoconf libmemcached libmemcached-devel

php 5.5 최신 버전 다운로드
wget https://www.php.net/distributions/php-5.5.38.tar.gz

php 압축해제
tar -xzvf php-5.5.38.tar.gz

cd php-5.5.38

sudo make clean
make distclean

./configure --help

./configure --with-apxs2 --with-mysql --with-mysqli --with-pdo-mysql --with-bz2 --with-curl --with-gd --with-jpeg-dir=/usr --with-png-dir=/usr --with-openssl --with-zlib --with-gettext --enable-mbstring --enable-fpm --enable-pcntl --enable-opcache --enable-soap --enable-sockets --enable-zip

./configure --prefix=/usr/local/php --with-config-file-path=/etc --with-apxs2=/usr/local/apache/bin/apxs --with-mysql=/usr/local/mysql --with-mysqli=/usr/local/mysql/bin/mysql_config --with-curl --disable-debug --enable-safe-mode  --enable-sockets --enable-sysvsem=yes --enable-sysvshm=yes --enable-ftp --enable-magic-quotes --with-ttf --enable-gd-native-ttf --enable-inline-optimization --enable-bcmath --with-zlib --with-gd --with-gettext --with-jpeg-dir=/usr --with-png-dir=/usr/lib --with-freetype-dir=/usr --with-libxml-dir=/usr --enable-exif --enable-sigchild --enable-mbstring --with-openssl

## 추가 설정
make distclean

-------------------------------------------------
# ZTS 모듈 사용
./configure --enable-sigchild --with-apxs2=/usr/bin/apxs --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --with-kerberos --with-openssl=shared --with-zlib=shared --enable-bcmath=shared --with-bz2=shared --with-curl=shared --enable-dom=shared --enable-calendar=shared --enable-exif=shared --enable-fileinfo=shared --enable-ftp=shared --with-gd=shared --with-iconv=shared --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext=shared --with-gmp=shared --with-mhash --enable-intl=shared --with-ldap=shared --enable-mbstring=shared --with-onig --enable-json=shared --with-mcrypt=shared --with-mysql=shared --enable-pcntl --enable-pdo=shared --with-pdo-mysql=shared --enable-mysqlnd=shared --with-pdo-sqlite=shared  --enable-phar=shared --enable-posix=shared --with-mysqli=shared --with-readline --enable-shmop=shared --enable-simplexml=shared --enable-sockets=shared --with-sqlite3=shared --with-snmp --enable-opcache --enable-soap=shared --enable-sysvmsg=shared --enable-sysvsem=shared --enable-sysvshm=shared --enable-tokenizer=shared --enable-wddx=shared --with-xsl=shared --enable-zip=shared --enable-zend-signals --with-freetype-dir --with-t1lib --with-xpm-dir --enable-xml=shared --with-xmlrpc=shared --enable-xmlreader=shared --enable-xmlwriter=shared --with-libdir=lib64 --enable-fpm --with-tsrm-pthreads --enable-maintainer-zts --prefix=/usr/local/php-zts

make && sudo make install

-rwxr-xr-x 1 root root 31717864 Nov 30 09:04 libphp5.so

sudo mv /usr/lib64/httpd/modules/libphp5.so /usr/lib64/httpd/modules/libphp-zts-5.5.so

-------------------------------------------------------
# 비ZTS 모듈 사용
make distclean

./configure --enable-sigchild --with-apxs2=/usr/bin/apxs --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --with-kerberos --with-openssl=shared --with-zlib=shared --enable-bcmath=shared --with-bz2=shared --with-curl=shared --enable-dom=shared --enable-calendar=shared --enable-exif=shared --enable-fileinfo=shared --enable-ftp=shared --with-gd=shared --with-iconv=shared --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext=shared --with-gmp=shared --with-mhash --enable-intl=shared --with-ldap=shared --enable-mbstring=shared --with-onig --enable-json=shared --with-mcrypt=shared --with-mysql=shared --enable-pcntl --enable-pdo=shared --with-pdo-mysql=shared --enable-mysqlnd=shared --with-pdo-sqlite=shared  --enable-phar=shared --enable-posix=shared --with-mysqli=shared --with-readline --enable-shmop=shared --enable-simplexml=shared --enable-sockets=shared --with-sqlite3=shared --with-snmp --enable-opcache --enable-soap=shared --enable-sysvmsg=shared --enable-sysvsem=shared --enable-sysvshm=shared --enable-tokenizer=shared --enable-wddx=shared --with-xsl=shared --enable-zip=shared --enable-zend-signals --with-freetype-dir --with-t1lib --with-xpm-dir --enable-xml=shared --with-xmlrpc=shared --enable-xmlreader=shared --enable-xmlwriter=shared --with-libdir=lib64 --enable-fpm --with-tsrm-pthreads --prefix=/usr/local/php

make && sudo make install

-rwxr-xr-x 1 root root 30986704 Nov 30 09:11 libphp5.so

sudo mv /usr/lib64/httpd/modules/libphp5.so /usr/lib64/httpd/modules/libphp-5.5.so

sudo systemctl restart httpd

------------------------------------------------------------

./configure --with-config-file-scan-dir=/etc/php-zts-5.5.d

------------------------------------------------------------
http/conf.d/php.conf

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>

AddType text/html .php

DirectoryIndex index.php

# session redis


------------------------------------------------------------
 10-php.conf

# 비ZTS 모듈 사용
<IfModule prefork.c>
    LoadModule php5_module /usr/lib64/httpd/modules/libphp-5.5.so
</IfModule>

# ZTS 모듈 사용
<IfModule !prefork.c>
    LoadModule php5_module /usr/lib64/httpd/modules/libphp-zts-5.5.so
</IfModule>

---------------------------------------------------------------

[ec2-user@amazonlinux php-5.5.38]$ sudo make install
Installing shared extensions:     /usr/local/lib/php/extensions/no-debug-zts-20121212/
Installing PHP CLI binary:        /usr/local/bin/
Installing PHP CLI man page:      /usr/local/php/man/man1/
Installing PHP FPM binary:        /usr/local/sbin/
Installing PHP FPM config:        /usr/local/etc/
Installing PHP FPM man page:      /usr/local/php/man/man8/
Installing PHP FPM status page:      /usr/local/php/php/fpm/
Installing PHP CGI binary:        /usr/local/bin/
Installing PHP CGI man page:      /usr/local/php/man/man1/
Installing build environment:     /usr/local/lib/php/build/
Installing header files:          /usr/local/include/php/
Installing helper programs:       /usr/local/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/php/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/lib/php/
[PEAR] Archive_Tar    - already installed: 1.4.0
[PEAR] Console_Getopt - already installed: 1.4.1
[PEAR] Structures_Graph- already installed: 1.1.1
[PEAR] XML_Util       - already installed: 1.3.0
[PEAR] PEAR           - already installed: 1.10.1
Wrote PEAR system config file at: /usr/local/etc/pear.conf
You may want to add: /usr/local/lib/php to your php.ini include_path
/home/ec2-user/php-5.5.38/build/shtool install -c ext/phar/phar.phar /usr/local/bin
ln -s -f phar.phar /usr/local/bin/phar
Installing PDO headers:          /usr/local/include/php/ext/pdo/

인스톨 확인
ls -la /usr/local/lib/php/extensions/no-debug-zts-20121212/

sudo mkdir /etc/php.d
sudo mkdir /usr/lib64/php
sudo mkdir /usr/lib64/php/modules

### php.ini 설정 수정
sudo cp php.ini-development /etc/php.ini
sudo vi /etc/php.ini
-------------------------------
date.timezone = "Asia/Tokyo"

extension_dir = "/usr/lib64/php/modules"

--------------------------

sudo cp sapi/fpm/php-fpm.conf /usr/local/etc/
sudo cp sapi/fpm/php-fpm.service /usr/lib/systemd/system/
sudo systemctl enable php-fpm


sudo systemctl start php-fpm

### 모듈 복사
sudo cp -pr /usr/local/lib/php/extensions/no-debug-zts-20121212/. /usr/lib64/php/modules/

### httpd.conf 설정 수정
sudo vi /etc/httpd/conf/httpd.conf
----------------

;LoadModule php5_module modules/libphp5.so

임시로 유저 변경
User ec2-user

変更後
<Directory />
    Options -Indexes +FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>


<Directory "/var/www/html">
    Options -Indexes +FollowSymLinks

    AllowOverride None

    Require all granted
</Directory>


<IfModule dir_module>
    DirectoryIndex index.html index.php index.php3
</IfModule>

<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options -Indexes +FollowSymLinks
    Require all granted
</Directory>

<IfModule mime_module>

    AddType application/x-httpd-php .php .html .html .inc
    AddType application/x-httpd-php-source .phps

    AddHandler type-map var

</IfModule>


Protocals h2c http/1.1
ProtocalsHonorOrder On

# to avoid proxy error 2022-12-20
SetEnv force-proxy-request-1.0 1
SetEnv proxy-nokeepalive 1
----------------

sudo systemctl restart httpd

cd /var/www/

sudo chown -R ec2-user:apache html

cd /var/www/html

echo "<?php phpinfo();" > phpinfo.php

# 자동실행

cd /etc/init.d/

ls -la

sudo systemctl status httpd
sudo systemctl status mariadb

// Apache
sudo systemctl enable httpd
// mariadb
sudo systemctl enable mariadb


# yum 업데이트가 안되는 경우


시스템이 "6 package(s) needed for security, out of 13 available"라는 메시지를 표시하고 있지만, 실제로 업데이트를 시도할 때 업데이트가 없다고 나타나는 경우, 몇 가지 가능한 원인이 있을 수 있습니다.

캐시된 메타데이터:

yum은 패키지 정보를 캐시에 저장합니다. 만약 이 캐시된 정보가 최신 상태가 아니라면, 실제로 사용 가능한 업데이트가 있음에도 불구하고 업데이트가 없다고 보고될 수 있습니다.
해결책: sudo yum clean all을 실행하여 캐시를 지운 후, sudo yum update를 다시 실행해 보세요.
리포지토리 문제:

사용 중인 리포지토리가 올바르게 구성되지 않았거나 문제가 있을 수 있습니다.
해결책: /etc/yum.repos.d/ 디렉토리 내의 리포지토리 설정 파일을 확인하고, 필요한 경우 올바른 리포지토리 설정으로 수정하세요.
네트워크 문제:

서버가 리포지토리에 접근하는 데 네트워크 문제가 있을 수 있습니다.
해결책: 네트워크 연결을 확인하고, 방화벽 또는 프록시 설정이 yum 작업을 방해하지 않는지 확인하세요.
버전 호환성:

특정 패키지가 현재 시스템 버전과 호환되지 않을 수 있습니다.
해결책: 시스템 버전과 호환되는 패키지 버전을 확인하세요.
수동으로 설치된 패키지:

시스템에 수동으로 설치된 패키지가 yum 데이터베이스에 올바르게 등록되지 않았을 수 있습니다.
해결책: 패키지 관리 상태를 확인하고 필요한 경우 재설치를 고려하세요.
버그 또는 일시적인 오류:

yum 자체의 버그 또는 일시적인 오류일 수 있습니다.
해결책: 시스템을 재부팅하거나 yum 업데이트 관련 포럼이나 문서에서 해결책을 찾아보세요.

# mysql 설치 확인

sudo yum list installed | grep mysql

sudo systemctl status mysqld

mysql --version

yum search mariadb

sudo yum install mariadb-server

--실행
sudo systemctl start mariadb
--시스템 부팅시 자동 실행
sudo systemctl enable mariadb

sudo mysqladmin password [password]

mysql -u root -p


외부 아이피 허용
use mysql

유저 추가
INSERT INTO user (Host, User) VALUES ('%', 'root');

모든 권한을 부여
GRANT ALL PRIVILEGES ON *.* TO {'ID'}@'%' IDENTIFIED BY {'PASSWORD'};

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'amazon';

FLUSH PRIVILEGES;

# git

sudo yum install git

git version


# 컴포저 설치

cd /home/ec2-user/

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

php composer-setup.php

php -r "unlink('composer-setup.php');"

sudo mv composer.phar /usr/local/bin/composer

# CakePHP 2.X

cd /var/www/html/

sudo wget https://github.com/cakephp/cakephp/archive/refs/tags/2.10.24.tar.gz

sudo tar -zxvf 2.10.24.tar.gz

sudo mv cakephp-2.10.24/ cakephp/

cd cakephp

cd app

sudo chmod -R 777 tmp

cd ..

sudo chmod -R 777 .

# redis 설치

sudo amazon-linux-extras install redis6

sudo yum install redis -y

sudo systemctl start redis

sudo systemctl enable redis

redis-server --version

redis-cli --version

# sudo yum install autoconf

wget https://codeload.github.com/phpredis/phpredis/zip/refs/tags/2.2.8

unzip phpredis-2.2.8.zip

cd phpredis-2.2.8

phpize

<!-- cd /home/ec2-user/php-5.5.38/ext/openssl

cp config0.m4 config.m4 -->

-------------------------------------
Configuring for:
PHP Api Version:         20121113
Zend Module Api No:      20121212
Zend Extension Api No:   220121212
----------------------------------------

sudo find / -name php-config

./configure

make && sudo make install

// sudo vi /usr/local/lib/php.ini

// extension=redis.so

ls -la /usr/local/lib/php/extensions/no-debug-zts-20121212/

redis.so 확인

sudo systemctl restart httpd

php -m

# memcache 설치

wget http://memcached.org/latest

tar -zxvf  memcached-1.6.22.tar.gz
cd  memcached-1.6.22
./configure && make && make test && sudo make install

---------------------------------------------
sudo tee /etc/sysconfig/memcached <<EOF
# These defaults will be used by every memcached instance, unless overridden
# by values in /etc/sysconfig/memcached.<port>
USER="nobody"
MAXCONN="1024"
CACHESIZE="64"
OPTIONS=""

# The PORT variable will only be used by memcached.service, not by
# memcached@xxxxx services, which will use the xxxxx
PORT="11211"
EOF
---------------------------------------------

yum install php-pecl-memcache

yum list | grep memcached
amazon-linux-extras list | grep memcached

amazon-linux-extras install -y memcached1.5
yum list installed | grep memcached

sudo service memcached start
---------------------------------------------
---------------------------------------------
# php-memcached 설치

https://github.com/php-memcached-dev/php-memcached/tree/2.2.0

unzip php-memcached-2.2.0.zip

cd php-memcached-2.2.0
phpize
./configure
make && sudo make install



ls -la /usr/local/lib/php/extensions/no-debug-zts-20121212/

memcached.so 확인

### 모듈 복사
sudo cp -pr /usr/local/lib/php/extensions/no-debug-zts-20121212/memcached.so /usr/lib64/php/modules/

sudo vi /etc/php.ini

php --ini


# igbinary 설치

cd ..

https://github.com/igbinary/igbinary/tree/2.0.8

unzip igbinary-2.0.8.zip

cd igbinary-2.0.8

phpize

./configure

make && sudo make install

ls -la /usr/local/lib/php/extensions/no-debug-zts-20121212/

igbinary.so 확인

sudo cp -pr /usr/local/lib/php/extensions/no-debug-zts-20121212/igbinary.so /usr/lib64/php/modules/

ls -la /usr/lib64/php/modules/

# php.d의 ini 파일 전부 복사

cd php.d/

sudo cp -pr *.* /etc/php.d/

sudo systemctl restart httpd

php -m

php --ini

## vendors

cd cakephp

composer install

composer require cakephp/debug_kit "^2.2.0"

# database.phpの作成
sudo cp /var/www/html/cakephp/app/Config/database.php.default /var/www/html/cakephp/app/Config/database.php


	public $default = array(
		'datasource' => 'Database/Mysql',
		'persistent' => false,
		'host' => '127.0.0.1',
		'login' => 'root',
		'password' => 'amazon',
		'database' => 'cakephp2',
		'prefix' => '',
		'encoding' => 'utf8',
	);

	public $test = array(
		'datasource' => 'Database/Mysql',
		'persistent' => false,
		'host' => '127.0.0.1',
		'login' => 'root',
		'password' => 'amazon',
		'database' => 'test_cakephp2',
		'prefix' => '',
		'encoding' => 'utf8',
	);

# vi /var/www/html/cakephp/app/Config/core.php

    225行目⇒文字列を適当に変更する
Configure::write('Security.salt', 'DYhG93b0qyJGKUSLGAFfsfIxfs2guVoUubWwvniR');

    230行目⇒乱数を適当に変更する
Configure::write('Security.cipherSeed', '76409709699974535424967584964');


app\Config\bootstrap.php

CakePlugin::load('DebugKit');


   # vi /var/www/html/cakephp/app/Controller/AppController.php

      class AppController extends Controller {
         public $components = array('DebugKit.Toolbar');　　※ここ
      }