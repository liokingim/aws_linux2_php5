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



---------------------------------------------------

PHP 경로 설정
설치 경로: ZTS와 비ZTS 버전의 PHP를 서로 다른 경로에 설치합니다. 예를 들어, ZTS 버전은 /usr/local/php-zts에, 비ZTS 버전은 /usr/local/php에 설치할 수 있습니다.
실행 파일: 각 설치 디렉토리 내에는 php 실행 파일이 포함됩니다. 예를 들어, ZTS 버전의 PHP 실행 파일은 /usr/local/php-zts/bin/php가 될 것입니다.

환경변수 설정
PATH 환경변수: 시스템의 PATH 환경변수에 PHP 실행 파일의 경로를 추가합니다. 이렇게 하면 터미널에서 php 명령을 입력했을 때 해당 버전의 PHP가 실행됩니다.

Bash 셸을 사용하는 경우, .bashrc 또는 .bash_profile 파일에 다음과 같이 추가할 수 있습니다:


export PATH="/usr/local/php/bin:$PATH"

위 예시는 비ZTS 버전의 PHP를 기본으로 설정합니다. ZTS 버전을 기본으로 설정하려면 경로를 /usr/local/php-zts/bin으로 변경합니다.

PHP_INI_SCAN_DIR: PHP 설정 파일(php.ini)을 로드할 추가 디렉토리를 지정하는 환경변수입니다. 필요한 경우 이 변수를 설정하여 특정 버전의 PHP에 대한 별도의 설정 디렉토리를 지정할 수 있습니다.

웹 서버 구성
Apache:
mod_php (비ZTS): Apache와 PHP를 통합하는 데 사용합니다. httpd.conf 또는 apache2.conf 파일에 LoadModule 지시어를 사용하여 PHP 모듈을 로드합니다.
php-fpm (ZTS): FastCGI 프로세스 매니저를 사용하여 PHP 요청을 처리합니다. ProxyPassMatch 또는 FilesMatch 지시어를 사용하여 php-fpm 서비스로 PHP 요청을 전달합니다.
Nginx:
Nginx를 사용하는 경우, php-fpm을 사용하여 PHP 요청을 처리합니다. Nginx 서버 블록 설정에서 location 지시어를 사용하여 php-fpm 풀에 PHP 요청을 전달합니다. ZTS와 비ZTS 버전에 대해 별도의 php-fpm 풀을 설정할 수 있습니다.
예시: Apache 구성
비ZTS 버전의 PHP (mod_php) 설정 예시:



LoadModule php5_module /usr/local/php/lib/httpd/modules/libphp5.so

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
ZTS 버전의 PHP (php-fpm) 설정 예시:


<FilesMatch \.php$>
    SetHandler "proxy:fcgi://localhost:9000"
</FilesMatch>

이 설정은 php-fpm이 localhost의 9000 포트에서 실행되고 있다고 가정합니다. 실제 포트와 주소는 php-fpm 구성에 따라 다를 수 있습니다.

주의사항
이러한 설정은 시스템의 복잡성을 증가시키며, 관리 및 디버깅을 더 어렵게 만들 수 있습니다.
보안과 성능을 위해 가능한 한 최신 버전의 PHP를 사용하는 것이 좋습니다.
모든 변경 사항을 적용한 후에는 웹 서버를 재시작해야 합니다.
시스템에



User
그러면 PHP ZTS와 비ZTS를 하나의 서버에서 동시에 동작하는 것인가
ChatGPT
네, 맞습니다. PHP의 ZTS(Zend Thread Safety) 버전과 비ZTS 버전을 하나의 서버에서 동시에 동작시키는 것이 가능합니다. 그러나 이런 설정은 일반적으로 권장되지 않으며, 특정한 상황과 필요에 따라 고려될 수 있습니다.

ZTS와 비ZTS 동시 운영의 의미
ZTS (Zend Thread Safety): PHP가 스레드 안전 모드로 컴파일되어, 멀티스레드 환경(예: 사용 중인 웹 서버가 멀티스레드를 사용하는 경우)에서 PHP 스크립트가 안전하게 실행될 수 있도록 합니다.
비ZTS (Non-Thread Safety): 스레드 안전 모드가 아닌 PHP의 컴파일 버전으로, 일반적으로 웹 서버와 PHP-FPM 또는 FastCGI를 통해 사용됩니다.
구현 방법
PHP 소스를 두 번 컴파일하여 각각 ZTS와 비ZTS 모드로 빌드합니다.
두 버전을 서로 다른 경로에 설치하여 구분합니다. 예를 들어, ZTS는 /usr/local/php-zts에, 비ZTS는 /usr/local/php에 설치할 수 있습니다.
웹 서버(Apache, Nginx 등)의 설정을 통해 각 버전의 PHP를 적절히 처리하도록 구성합니다. 예를 들어, Apache에서는 mod_php로 비ZTS 버전을, php-fpm으로 ZTS 버전을 처리할 수 있습니다.
주의사항
이러한 설정은 시스템의 복잡성을 증가시키고 관리를 어렵게 만듭니다.
멀티스레드 환경이 아닌 경우, ZTS 버전은 추가적인 성능 오버헤드를 가질 수 있습니다.
각 버전의 PHP 설정(php.ini)과 확장들을 따로 관리해야 합니다.
보안 및 성능 최적화를 위해 최신 버전의 PHP를 사용하는 것이 좋습니다.
특정 확장 또는 라이브러리가 ZTS와 비ZTS 모드에서 각각 다르게 동작할 수 있으므로 주의가 필요합니다.
결론
하나의 서버에서 PHP의 ZTS와 비ZTS 버전을 동시에 운영하는 것은 가능하지만, 일반적인 사용 사례에서는 필요하지 않을 수 있으며, 특별한 요구 사항이나 성능, 스레드 안전성과 관련된 이유가 있을 때만 고려해야 합니다.

User
아파치 서버에 멀티스레드 환경이라는 것은 어떻게 판단하지
ChatGPT
아파치 서버에서 "멀티스레드 환경"이라는 것을 판단하는 기준은 주로 서버가 사용하는 멀티 프로세싱 모듈(MPM)의 종류에 따라 달라집니다. Apache 서버에는 여러 가지 MPM이 있으며, 각각은 웹 요청을 처리하는 방식이 다릅니다.

주요 Apache MPM 유형
Prefork MPM:

Prefork MPM은 멀티스레드가 아닌 멀티프로세스 방식을 사용합니다.
각 HTTP 요청은 별도의 자식 프로세스에 의해 처리됩니다.
이 방식은 스레드 안전하지 않은 애플리케이션과 라이브러리에 적합합니다.
Prefork MPM은 멀티스레드 환경이라고 보기 어렵습니다.
Worker MPM:

Worker MPM은 멀티스레드와 멀티프로세스를 혼합한 방식을 사용합니다.
메인 프로세스가 여러 자식 프로세스를 생성하고, 각 자식 프로세스는 여러 스레드를 관리하여 요청을 처리합니다.
Worker MPM은 멀티스레드 환경에 해당합니다.
Event MPM:

Event MPM도 Worker MPM과 유사하지만, 더 효율적인 요청 처리와 네트워크 통신을 위해 "이벤트 기반" 접근 방식을 사용합니다.
이 모듈 역시 멀티스레드 환경으로 분류됩니다.
멀티스레드 환경 판단 방법
현재 사용 중인 MPM 확인:

아파치의 현재 MPM을 확인하려면, 터미널에서 다음 명령어를 실행합니다:
bash
Copy code
apachectl -V | grep MPM
이 명령은 현재 사용 중인 MPM에 대한 정보를 제공합니다.
Apache 구성 파일 검토:

Apache의 구성 파일(httpd.conf 또는 apache2.conf)을 검토하여 LoadModule 지시어를 통해 어떤 MPM이 로드되고 있는지 확인합니다.
운영 체제 및 배포판 고려:

일부 운영 체제 또는 배포판에서는 기본적으로 특정 MPM을 사용할 수도 있습니다. 예를 들어, Ubuntu에서는 기본적으로 Event MPM을 사용합니다.
주의사항
스레드 안전성: 멀티스레드 MPM을 사용하는 경우, 스레드 안전(thread-safe)한 PHP 빌드(예: PHP ZTS 버전)가 필요할 수 있습니다.
성능 및 자원 사용: MPM 선택은 성능, 메모리 사용량, 그리고 서버의 응답 시간에 영향을 미칠 수 있습니다. 선택하는 MPM은 서버의 하드웨어 사양과 트래픽 특성에 따라 달라져야 합니다.
모듈 호환성: 일부 Apache 모듈은 특정 MPM과 더 잘 호환될 수 있으므로, 사용하고자 하는 모듈과 MPM 사이의 호환성도 고려해야 합니다.
User
그러면 aws linux 2버전의 아파치는 멀티스레드인가
ChatGPT
Amazon Linux 2에서 Apache 서버의 멀티스레드 지원 여부는 해당 서버에서 사용 중인 멀티 프로세싱 모듈(MPM)에 따라 달라집니다. 일반적으로 Apache 서버는 여러 MPM 중 하나를 사용하며, 이 중 'Worker'와 'Event' MPM은 멀티스레드를 지원합니다. 반면, 'Prefork' MPM은 멀티스레드를 지원하지 않고 멀티프로세스 방식을 사용합니다.

확인 방법
Amazon Linux 2에서 실행 중인 Apache 서버의 MPM을 확인하려면, 다음 단계를 수행할 수 있습니다:

터미널을 열고 아래의 명령어를 실행합니다:

apachectl -V | grep MPM

이 명령은 현재 Apache에서 사용 중인 MPM의 정보를 보여줍니다.

결과 확인:

Server MPM: Prefork라고 표시되면 멀티프로세스, 멀티스레드가 아닌 환경입니다.
Server MPM: Worker 또는 Server MPM: Event라고 표시되면 멀티스레드 환경입니다.
추가 정보
Worker MPM: 이 모듈은 멀티프로세스와 멀티스레드를 혼합한 방식을 사용합니다. 각 프로세스는 여러 스레드를 생성하여 HTTP 요청을 처리합니다.
Event MPM: Worker와 유사하지만, 네트워크 연결을 효율적으로 관리하기 위해 '이벤트 기반' 접근 방식을 사용합니다. 이는 보다 높은 트래픽 상황에서 성능을 향상시킵니다.
Prefork MPM: 각 HTTP 요청을 별도의 프로세스에서 처리합니다. 이 방식은 스레드 안전하지 않은 애플리케이션에 적합합니다.
중요 사항
Amazon Linux 2의 기본 Apache 구성은 MPM의 종류에 따라 다를 수 있으며, 필요에 따라 변경할 수 있습니다.
MPM을 변경하거나 구성을 수정한 후에는 Apache 서비스를 재시작해야 새 설정이 적용됩니다.
스레드 안전성이 중요한 경우(예: PHP와 같은 언어 사용 시), 적절한 MPM을 선택하는 것이 중요합니다.

---------------------------------------------

웹 서버 구성에서 아파치(Apache)를 사용하여 PHP의 ZTS(Zend Thread Safety)와 비ZTS 버전을 동시에 운영하려면, 몇 가지 추가 설정이 필요합니다. 아래에는 Apache를 사용하여 PHP의 ZTS 및 비ZTS 버전을 설정하는 방법에 대한 예시를 제공합니다.

Apache 설정 예시
PHP-FPM 사용 (ZTS 버전을 위해):

PHP의 ZTS 버전은 php-fpm을 사용하여 구성하는 것이 일반적입니다.
php-fpm 풀 설정을 생성하고, 각 PHP ZTS 버전의 php-fpm 서비스를 구성합니다.
예를 들어, /etc/php-fpm.d/www-zts.conf 파일을 만들고 해당 PHP ZTS 버전에 맞게 구성합니다.
php-fpm 서비스를 시작합니다.
Apache mod_php 사용 (비ZTS 버전을 위해):

PHP의 비ZTS 버전은 mod_php 모듈을 사용하여 Apache와 직접 통합합니다.
Apache 구성 파일에 mod_php 모듈을 로드하는 구문을 추가합니다.
예를 들어, Apache의 httpd.conf 또는 apache2.conf 파일에 다음을 추가합니다:
apache
Copy code
LoadModule php5_module modules/libphp5.so
Virtual Host 또는 Directory 지시어 설정:

Apache의 VirtualHost 또는 Directory 지시어를 사용하여, ZTS와 비ZTS 버전을 처리할 방식을 구분합니다.
비ZTS 설정 예시:

apache
Copy code
<VirtualHost *:80>
    ServerName non-zts.example.com
    DocumentRoot "/path/to/non-zts/document/root"

    <Directory "/path/to/non-zts/document/root">
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
        AddType application/x-httpd-php .php
    </Directory>
</VirtualHost>
ZTS 설정 예시 (PHP-FPM 사용):

apache
Copy code
<VirtualHost *:80>
    ServerName zts.example.com
    DocumentRoot "/path/to/zts/document/root"

    <Directory "/path/to/zts/document/root">
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:fcgi://localhost:9000"
    </FilesMatch>
</VirtualHost>
이 예시에서 9000은 PHP-FPM의 ZTS 버전이 사용하는 포트 번호입니다. 실제 포트 번호는 www-zts.conf 파일에서 설정한 값과 일치해야 합니다.

주의사항
이 설정은 예시일 뿐이며, 실제 환경에 맞게 조정해야 합니다.
Apache와 PHP-FPM을 적절히 설정하고 재시작해야 적용됩니다.
보안 설정 및 성능 최적화를 위해 추가적인 설정이 필요할 수 있습니다.
ZTS와 비ZTS PHP 버전을 동시에 운영하는 것은 관리가 복잡해질 수 있으므로, 반드시 필요한 경우에만 구성하는 것이 좋습니다.

------------------------------------------------------------

php-fpm (PHP FastCGI Process Manager)는 PHP 스크립트를 웹 서버와 효율적으로 통합하기 위한 고급 및 매우 유연한 프로세스 관리자입니다. 원래는 PHP 커뮤니티에서 독립적으로 개발되었지만, PHP 5.3.3 버전 이후로 PHP의 공식 일부가 되었습니다.

php-fpm의 주요 특징 및 장점
성능과 확장성:

php-fpm은 FastCGI 프로토콜을 사용하여 PHP 요청을 처리합니다. 이는 웹 서버와 PHP 간의 통신을 최적화하여 웹 애플리케이션의 성능을 향상시킵니다.
고부하 환경에서 탁월한 성능과 확장성을 제공합니다.
고급 프로세스 관리:

동적 프로세스 관리를 통해 서버 부하에 따라 PHP 프로세스의 수를 조절합니다. 이는 서버 자원을 효율적으로 사용하도록 돕습니다.
프로세스를 시작, 정지, 그리고 재시작할 수 있는 능력을 제공합니다.
안정성과 유연성:

문제가 있는 자식 프로세스를 자동으로 복구하여 웹 서비스의 안정성을 유지합니다.
다양한 운영 환경에 맞게 구성이 가능합니다.
세부적인 구성 가능:

php-fpm은 pool 설정을 통해 다양한 웹사이트나 애플리케이션에 대해 각각 다른 환경을 설정할 수 있습니다. 각 pool은 별도의 사용자, 그룹, 프로세스 수, PHP 설정 등을 가질 수 있습니다.
사용 방법
php-fpm은 일반적으로 Apache 또는 Nginx 같은 웹 서버와 함께 사용됩니다.
Apache와 함께 사용할 때는 mod_proxy_fcgi 모듈을 통해 연결합니다.
Nginx와 함께 사용할 때는 fastcgi_pass 지시어를 사용하여 Nginx 설정 파일에서 php-fpm으로 요청을 전달합니다.
설정
php-fpm의 설정 파일은 일반적으로 /etc/php-fpm.d/ 디렉토리에 위치하며, 여기에서 pool 정의 및 기타 설정을 할 수 있습니다.

예시
nginx
Copy code
location ~ \.php$ {
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
위의 Nginx 설정 예시는 php-fpm이 127.0.0.1의 9000 포트에서 수신하고 있다고 가정합니다.

주의사항
php-fpm의 구성은 웹 애플리케이션의 요구사항에 따라 달라질 수 있으며, 프로세스 수, 메모리 사용량 등을 적절히 조절해야 합니다.
php-fpm은 웹 서버와 PHP 사이의 효율적인 연결을 위해 광범위하게 사용되며, 대규모 웹 애플리케이션 및 서비스에서 널리 사용됩니다.
User
아파치에서의 설정도 알려줘
ChatGPT
Apache 서버에서 PHP-FPM (FastCGI Process Manager)을 사용하기 위한 설정은 여러 단계를 거치게 됩니다. 이 설정은 Apache가 PHP 스크립트를 처리할 때 PHP-FPM을 통해 이루어지도록 합니다. 아래는 Apache에서 PHP-FPM을 설정하는 기본적인 절차입니다:

1. 필요한 모듈 설치
Apache에서 PHP-FPM을 사용하기 위해 필요한 모듈은 mod_proxy_fcgi입니다. 대부분의 최신 Apache 서버에는 이미 이 모듈이 포함되어 있지만, 없을 경우 설치해야 합니다.

2. PHP-FPM 서비스 구성
PHP-FPM은 .conf 파일을 통해 구성됩니다. 일반적으로 이 파일은 /etc/php/7.x/fpm/pool.d/ (여기서 7.x는 PHP 버전)에 위치합니다. 각각의 웹사이트 또는 애플리케이션에 대해 별도의 "pool"을 설정할 수 있으며, 이는 각기 다른 포트 또는 소켓을 사용할 수 있습니다.

예를 들어, www.conf 파일에서 다음과 같이 설정할 수 있습니다:

ini
Copy code
[www]
user = www-data
group = www-data
listen = 127.0.0.1:9000
pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
여기서 listen 지시어는 PHP-FPM이 연결을 수신할 주소와 포트를 정의합니다.

3. Apache 가상 호스트 구성
Apache의 가상 호스트 설정에서 PHP-FPM으로 요청을 전달하도록 설정합니다. mod_proxy_fcgi 모듈과 ProxyPassMatch 지시어를 사용하여 .php 파일에 대한 요청을 PHP-FPM에 전달합니다.

/etc/apache2/sites-available/000-default.conf 파일 또는 다른 가상 호스트 구성 파일에서 다음과 같이 설정할 수 있습니다:

apache
Copy code
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:fcgi://127.0.0.1:9000"
    </FilesMatch>
</VirtualHost>
이 설정은 모든 .php 파일에 대한 요청을 127.0.0.1의 9000 포트에서 수신하는 PHP-FPM으로 전달합니다.

4. Apache 및 PHP-FPM 재시작
설정을 적용한 후, Apache 서버와 PHP-FPM 서비스를 재시작해야 합니다:

bash
Copy code
sudo systemctl restart apache2
sudo systemctl restart php7.x-fpm
여기서 php7.x-fpm은 실제 PHP 버전에 따라 달라집니다.

주의사항
이 설정은 Apache 서버와 PHP-FPM 사이의 연결을 설정하는 기본적인 예시입니다. 실제 환경에 맞게 조정이 필요할 수 있습니다.
보안 및 성능 최적화를 위해 추가적인 설정이 필요할 수 있습니다.
PHP-FPM 설정(pool 설정 등)은 웹 애플리케이션의 요구사항에 따라 다를 수 있습니다.

