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


php 설치에 필요한 모듈 설치
sudo yum install -y gcc make libxml2-devel openssl-devel bzip2-devel libcurl-devel libjpeg-devel libpng-devel libicu-devel libmcrypt-devel libxslt-devel httpd-devel

sudo yum -y install gcc-c++ libxml2-devel openssl-devel bzip2-devel libcurl-devel curl-devel libpng-devel libjpeg-devel freetype-devel t1lib-devel gmp-devel  libicu-devel openldap-devel libmcrypt-devel mysql-devel readline-devel net-snmp-devel  libxslt-devel libXpm-devel libevent libevent-devel httpd-devel

php 5.5 최신 버전 다운로드
wget https://www.php.net/distributions/php-5.5.38.tar.gz

php 압축해제
tar -xzvf php-5.5.38.tar.gz

cd php-5.5.38

make clean

./configure --with-apxs2 --with-mysql --with-mysqli --with-pdo-mysql --with-bz2 --with-curl --with-gd --with-jpeg-dir=/usr --with-png-dir=/usr --with-openssl --with-zlib --with-gettext --enable-mbstring --enable-fpm --enable-pcntl --enable-opcache --enable-soap --enable-sockets --enable-zip

## 추가 설정
./configure --enable-sigchild --with-apxs2 --with-kerberos --with-openssl --with-zlib --enable-bcmath --with-bz2 --with-curl --enable-exif --enable-ftp --with-gd  --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext --with-gmp --with-mhash --enable-intl --with-ldap --enable-mbstring --with-onig --with-mcrypt --with-mysql --enable-pcntl --with-pdo-mysql --with-mysqli --with-readline --enable-shmop --with-snmp --enable-soap --enable-sockets --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-wddx --with-xsl --enable-zip --enable-zend-signals --with-freetype-dir --with-t1lib --with-xpm-dir --with-libdir=lib64 --enable-fpm --with-tsrm-pthreads --enable-maintainer-zts

make && sudo make install


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


### php.ini 설정 수정
sudo cp php.ini-development /usr/local/lib/php.ini
sudo vi /usr/local/lib/php.ini
-------------------------------
date.timezone = "Asia/Tokyo"

;extension=mysqli.so
;extension=pdo_mysql.so
--------------------------

sudo cp sapi/fpm/php-fpm.conf /usr/local/etc/
sudo cp sapi/fpm/php-fpm.service /usr/lib/systemd/system/
sudo systemctl enable php-fpm
sudo systemctl start php-fpm

### httpd.conf 설정 수정
sudo vi /etc/httpd/conf/httpd.conf
----------------

<IfModule dir_module>
    DirectoryIndex index.html index.php index.php3
</IfModule>


;LoadModule php5_module modules/libphp5.so


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


<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options -Indexes +FollowSymLinks
    Require all granted
</Directory>

<IfModule mime_module>


    AddHandler type-map var

</IfModule>

AddType application/x-httpd-php .php .html .html .inc
AddType application/x-httpd-php-source .phps

Protocals h2c http/1.1
ProtocalsHonorOrder On

# to avoid proxy error 2022-12-20
SetEnv force-proxy-request-1.0 1
SetEnv proxy-nokeepalive 1
----------------

sudo systemctl restart httpd

cd /var/www/html

echo "<?php phpinfo();" > test.php

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
sudo yum install autoconf

sudo /usr/local/bin/phpize

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

sudo vi /usr/local/lib/php.ini

extension=redis.so

sudo systemctl restart httpd

php -m

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