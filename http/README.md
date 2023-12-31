## apache install

sudo yum install -y wget perl zlib-devel gcc gcc-c++ pcre-devel libxml2-devel openssl-devel expat-devel cmake git automake autoconf libtool

yum list |grep http
yum list |grep nghttp2

ps aux |grep http

firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload


sudo make clean
make distclean

## apr install
tar -zxvf apr-1.7.4.tar.gz
cd apr-1.7.4
./configure
make && sudo make install
cd ..

## apr-util install
tar -zxvf apr-util-1.6.3.tar.gz
cd apr-util-1.6.3
./configure --with-apr=/usr/local/apr
make && sudo make install
cd ..

## nghttp2 install
tar -zxvf nghttp2-1.58.0.tar.gz
cd nghttp2-1.58.0
./configure
make && sudo make install
cd ..


## OpenSSL install
tar -zxvf openssl-1.1.1w.tar.gz
cd openssl-1.1.1w
./config shared zlib-dynamic --prefix=/usr/local/ssl
make && sudo make install
cd ..

## pcre 설치
tar zxvf pcre2-10.42.tar.gz
cd pcre2-10.42
./configure --prefix=/usr/local/pcre2
make && sudo make install

tar -zxvf pcre-8.45.tar.gz
cd pcre-8.45
./configure --prefix=/usr/local/pcre
make && sudo make install

# Lynx 설치

yum install -y lynx.x86_64

## httpd install

tar zxvf httpd-2.4.58.tar.gz

cd httpd-2.4.58

./configure --prefix=/opt/apache --exec-prefix=/usr --bindir=/usr/bin --sbindir=/usr/sbin --mandir=/usr/share/man --libdir=/usr/lib64 --sysconfdir=//opt/apache/conf --includedir=/usr/include --datadir=/var/www --localstatedir=/var --with-mpm=prefork --enable-http2 --with-nghttp2=/usr/local --with-ssl=/usr/local/ssl --with-pcre=/usr/local/pcre

make
sudo make install

cp -pr /tmp/httpd-2.4.58/modules/http2/.libs/mod_http2.so /usr/modules/mod_http2.so

sudo apachectl start

 Apache 구성
설치가 완료된 후 Apache의 구성 파일(httpd.conf 또는 해당하는 vhost 파일)에서 HTTP/2를 활성화합니다.

Protocols h2 http/1.1


5. Apache 재시작
설정을 적용하기 위해 Apache 서버를 재시작합니다.

apachectl configtest

sudo apachectl restart


# 자체 서명된 인증서 생성
cd /opt/apache
mkdir ssl
cd /opt/apache/ssl

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout localhost.key -out localhost.crt

sudo cp /tmp/localhost.crt /opt/apache/ssl/
sudo cp /tmp/localhost.key /opt/apache/ssl/

# httpd.conf
mod_socache_shmcb
mod_dav.so
mod_dav_fs
mod_auth_digest

LoadModule auth_digest_module /usr/modules/mod_auth_digest.so
LoadModule socache_shmcb_module /usr/modules/mod_socache_shmcb.so
LoadModule ssl_module /usr/modules/mod_ssl.so
LoadModule dav_module /usr/modules/mod_dav.so
LoadModule dav_fs_module /usr/modules/mod_dav_fs.so
LoadModule http2_module /usr/modules/mod_http2.so

cat httpd-dav.conf
cat httpd-ssl.conf |grep SSLSessionCache


# httpd-ssl.conf

<VirtualHost *:443>
    SSLEngine on
    Protocols h2 http/1.1

    # 기타 SSL 관련 구성
    ServerName localhost:443
    DocumentRoot "/var/www/htdocs"

    SSLCertificateFile "/opt/apache/ssl/localhost.crt"
    SSLCertificateKeyFile "/opt/apache/ssl/localhost.key"

    <Directory "/var/www/htdocs">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

# vi httpd-vhosts.conf
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "/var/www/htdocs"
    ServerName localhost:80
    ServerAlias www.localhost-test.com
    ErrorLog "/var/logs/localhost-test.com-error_log"
    CustomLog "/var/logs/localhost-test.com-access_log" common
</VirtualHost>



# apachectl status 상태확인
httpd.conf

<Location "/server-status">
    SetHandler server-status
    Require host localhost
</Location>

apachectl status

# Apache용 systemd 서비스 파일 생성

vi /etc/systemd/system/httpd.service

[Unit]
Description=The Apache HTTP Server
After=network.target

[Service]
Type=forking
ExecStart=/usr/sbin/apachectl start
ExecReload=/usr/sbin/apachectl graceful
ExecStop=/usr/sbin/apachectl stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target

#

systemctl daemon-reload

sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd



https://www.tunetheweb.com/performance/http2/


httpd.conf 수정: httpd.conf 파일을 수정하여 conf.d 디렉토리 내의 모든 설정 파일을 포함하도록 합니다. httpd.conf 파일의 맨 아래에 다음 라인을 추가합니다:

mkdir /usr/local/apache/conf/conf.d

IncludeOptional conf/conf.d/*.conf

mkdir /path/to/apache/conf/conf.module.d

IncludeOptional conf/conf.module.d/*.conf


drwxr-xr-x.   2 root root   37 Dec 22 00:56 conf
drwxr-xr-x.   2 root root   82 Dec 22 00:56 conf.d
drwxr-xr-x.   2 root root  146 Dec 22 00:56 conf.modules.d
lrwxrwxrwx.   1 root root   19 Dec 22 00:56 logs -> ../../var/log/httpd
lrwxrwxrwx.   1 root root   29 Dec 22 00:56 modules -> ../../usr/lib64/httpd/modules
lrwxrwxrwx.   1 root root   10 Dec 22 00:56 run -> /run/httpd


/etc/httpd/conf/httpd.conf
/etc/httpd/conf/magic

/etc/httpd/conf.d/autoindex.conf
/etc/httpd/conf.d/README
/etc/httpd/conf.d/userdir.conf
/etc/httpd/conf.d/welcome.conf

/etc/httpd/conf.modules.d/00-base.conf
/etc/httpd/conf.modules.d/00-dav.conf
/etc/httpd/conf.modules.d/00-lua.conf
/etc/httpd/conf.modules.d/00-mpm.conf
/etc/httpd/conf.modules.d/00-proxy.conf
/etc/httpd/conf.modules.d/00-systemd.conf
/etc/httpd/conf.modules.d/01-cgi.conf


/usr/lib64/httpd/modules/mod_access_compat.so
/usr/lib64/httpd/modules/mod_actions.so
/usr/lib64/httpd/modules/mod_alias.so
/usr/lib64/httpd/modules/mod_allowmethods.so
/usr/lib64/httpd/modules/mod_asis.so
/usr/lib64/httpd/modules/mod_auth_basic.so
/usr/lib64/httpd/modules/mod_auth_digest.so
/usr/lib64/httpd/modules/mod_authn_anon.so
/usr/lib64/httpd/modules/mod_authn_core.so
/usr/lib64/httpd/modules/mod_authn_dbd.so
/usr/lib64/httpd/modules/mod_authn_dbm.so
/usr/lib64/httpd/modules/mod_authn_file.so
/usr/lib64/httpd/modules/mod_authn_socache.so
/usr/lib64/httpd/modules/mod_authz_core.so
/usr/lib64/httpd/modules/mod_authz_dbd.so
/usr/lib64/httpd/modules/mod_authz_dbm.so
/usr/lib64/httpd/modules/mod_authz_groupfile.so
/usr/lib64/httpd/modules/mod_authz_host.so
/usr/lib64/httpd/modules/mod_authz_owner.so
/usr/lib64/httpd/modules/mod_authz_user.so
/usr/lib64/httpd/modules/mod_autoindex.so
/usr/lib64/httpd/modules/mod_buffer.so
/usr/lib64/httpd/modules/mod_cache.so
/usr/lib64/httpd/modules/mod_cache_disk.so
/usr/lib64/httpd/modules/mod_cache_socache.so
/usr/lib64/httpd/modules/mod_cgi.so
/usr/lib64/httpd/modules/mod_cgid.so
/usr/lib64/httpd/modules/mod_charset_lite.so
/usr/lib64/httpd/modules/mod_data.so
/usr/lib64/httpd/modules/mod_dav.so
/usr/lib64/httpd/modules/mod_dav_fs.so
/usr/lib64/httpd/modules/mod_dav_lock.so
/usr/lib64/httpd/modules/mod_dbd.so
/usr/lib64/httpd/modules/mod_deflate.so
/usr/lib64/httpd/modules/mod_dialup.so
/usr/lib64/httpd/modules/mod_dir.so
/usr/lib64/httpd/modules/mod_dumpio.so
/usr/lib64/httpd/modules/mod_echo.so
/usr/lib64/httpd/modules/mod_env.so
/usr/lib64/httpd/modules/mod_expires.so
/usr/lib64/httpd/modules/mod_ext_filter.so
/usr/lib64/httpd/modules/mod_file_cache.so
/usr/lib64/httpd/modules/mod_filter.so
/usr/lib64/httpd/modules/mod_headers.so
/usr/lib64/httpd/modules/mod_heartbeat.so
/usr/lib64/httpd/modules/mod_heartmonitor.so
/usr/lib64/httpd/modules/mod_include.so
/usr/lib64/httpd/modules/mod_info.so
/usr/lib64/httpd/modules/mod_lbmethod_bybusyness.so
/usr/lib64/httpd/modules/mod_lbmethod_byrequests.so
/usr/lib64/httpd/modules/mod_lbmethod_bytraffic.so
/usr/lib64/httpd/modules/mod_lbmethod_heartbeat.so
/usr/lib64/httpd/modules/mod_log_config.so
/usr/lib64/httpd/modules/mod_log_debug.so
/usr/lib64/httpd/modules/mod_log_forensic.so
/usr/lib64/httpd/modules/mod_logio.so
/usr/lib64/httpd/modules/mod_lua.so
/usr/lib64/httpd/modules/mod_macro.so
/usr/lib64/httpd/modules/mod_mime.so
/usr/lib64/httpd/modules/mod_mime_magic.so
/usr/lib64/httpd/modules/mod_mpm_event.so
/usr/lib64/httpd/modules/mod_mpm_prefork.so
/usr/lib64/httpd/modules/mod_mpm_worker.so
/usr/lib64/httpd/modules/mod_negotiation.so
/usr/lib64/httpd/modules/mod_proxy.so
/usr/lib64/httpd/modules/mod_proxy_ajp.so
/usr/lib64/httpd/modules/mod_proxy_balancer.so
/usr/lib64/httpd/modules/mod_proxy_connect.so
/usr/lib64/httpd/modules/mod_proxy_express.so
/usr/lib64/httpd/modules/mod_proxy_fcgi.so
/usr/lib64/httpd/modules/mod_proxy_fdpass.so
/usr/lib64/httpd/modules/mod_proxy_ftp.so
/usr/lib64/httpd/modules/mod_proxy_http.so
/usr/lib64/httpd/modules/mod_proxy_scgi.so
/usr/lib64/httpd/modules/mod_proxy_wstunnel.so
/usr/lib64/httpd/modules/mod_ratelimit.so
/usr/lib64/httpd/modules/mod_reflector.so
/usr/lib64/httpd/modules/mod_remoteip.so
/usr/lib64/httpd/modules/mod_reqtimeout.so
/usr/lib64/httpd/modules/mod_request.so
/usr/lib64/httpd/modules/mod_rewrite.so
/usr/lib64/httpd/modules/mod_sed.so
/usr/lib64/httpd/modules/mod_setenvif.so
/usr/lib64/httpd/modules/mod_slotmem_plain.so
/usr/lib64/httpd/modules/mod_slotmem_shm.so
/usr/lib64/httpd/modules/mod_socache_dbm.so
/usr/lib64/httpd/modules/mod_socache_memcache.so
/usr/lib64/httpd/modules/mod_socache_shmcb.so
/usr/lib64/httpd/modules/mod_speling.so
/usr/lib64/httpd/modules/mod_status.so
/usr/lib64/httpd/modules/mod_substitute.so
/usr/lib64/httpd/modules/mod_suexec.so
/usr/lib64/httpd/modules/mod_systemd.so
/usr/lib64/httpd/modules/mod_unique_id.so
/usr/lib64/httpd/modules/mod_unixd.so
/usr/lib64/httpd/modules/mod_userdir.so
/usr/lib64/httpd/modules/mod_usertrack.so
/usr/lib64/httpd/modules/mod_version.so
/usr/lib64/httpd/modules/mod_vhost_alias.so
/usr/lib64/httpd/modules/mod_watchdog.so


/run/httpd/htcacheclean/


----------------------------------------

## centos 8

httpd -V
Server version: Apache/2.4.37 (CentOS Stream)
Server built:   Aug 17 2023 14:07:36
Server's Module Magic Number: 20120211:83
Server loaded:  APR 1.6.3, APR-UTIL 1.6.1
Compiled using: APR 1.6.3, APR-UTIL 1.6.1
Architecture:   64-bit
Server MPM:     event
  threaded:     yes (fixed thread count)
    forked:     yes (variable process count)
Server compiled with....
 -D APR_HAS_SENDFILE
 -D APR_HAS_MMAP
 -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
 -D APR_USE_SYSVSEM_SERIALIZE
 -D APR_USE_PTHREAD_SERIALIZE
 -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
 -D APR_HAS_OTHER_CHILD
 -D AP_HAVE_RELIABLE_PIPED_LOGS
 -D DYNAMIC_MODULE_LIMIT=256
 -D HTTPD_ROOT="/etc/httpd"
 -D SUEXEC_BIN="/usr/sbin/suexec"
 -D DEFAULT_PIDLOG="run/httpd.pid"
 -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
 -D DEFAULT_ERRORLOG="logs/error_log"
 -D AP_TYPES_CONFIG_FILE="conf/mime.types"
 -D SERVER_CONFIG_FILE="conf/httpd.conf"


[root@localhost conf.modules.d]# httpd -D DUMP_MODULES
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using localhost.localdomain. Set the 'ServerName' directive globally to suppress this message
Loaded Modules:
 core_module (static)
 so_module (static)
 http_module (static)
 access_compat_module (shared)
 actions_module (shared)
 alias_module (shared)
 allowmethods_module (shared)
 auth_basic_module (shared)
 auth_digest_module (shared)
 authn_anon_module (shared)
 authn_core_module (shared)
 authn_dbd_module (shared)
 authn_dbm_module (shared)
 authn_file_module (shared)
 authn_socache_module (shared)
 authz_core_module (shared)
 authz_dbd_module (shared)
 authz_dbm_module (shared)
 authz_groupfile_module (shared)
 authz_host_module (shared)
 authz_owner_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 brotli_module (shared)
 cache_module (shared)
 cache_disk_module (shared)
 cache_socache_module (shared)
 data_module (shared)
 dbd_module (shared)
 deflate_module (shared)
 dir_module (shared)
 dumpio_module (shared)
 echo_module (shared)
 env_module (shared)
 expires_module (shared)
 ext_filter_module (shared)
 filter_module (shared)
 headers_module (shared)
 include_module (shared)
 info_module (shared)
 log_config_module (shared)
 logio_module (shared)
 macro_module (shared)
 mime_magic_module (shared)
 mime_module (shared)
 negotiation_module (shared)
 remoteip_module (shared)
 reqtimeout_module (shared)
 request_module (shared)
 rewrite_module (shared)
 setenvif_module (shared)
 slotmem_plain_module (shared)
 slotmem_shm_module (shared)
 socache_dbm_module (shared)
 socache_memcache_module (shared)
 socache_shmcb_module (shared)
 status_module (shared)
 substitute_module (shared)
 suexec_module (shared)
 unique_id_module (shared)
 unixd_module (shared)
 userdir_module (shared)
 version_module (shared)
 vhost_alias_module (shared)
 watchdog_module (shared)
 dav_module (shared)
 dav_fs_module (shared)
 dav_lock_module (shared)
 lua_module (shared)
 mpm_event_module (shared)
 proxy_module (shared)
 lbmethod_bybusyness_module (shared)
 lbmethod_byrequests_module (shared)
 lbmethod_bytraffic_module (shared)
 lbmethod_heartbeat_module (shared)
 proxy_ajp_module (shared)
 proxy_balancer_module (shared)
 proxy_connect_module (shared)
 proxy_express_module (shared)
 proxy_fcgi_module (shared)
 proxy_fdpass_module (shared)
 proxy_ftp_module (shared)
 proxy_http_module (shared)
 proxy_hcheck_module (shared)
 proxy_scgi_module (shared)
 proxy_uwsgi_module (shared)
 proxy_wstunnel_module (shared)
 systemd_module (shared)
 cgid_module (shared)
 http2_module (shared)
 proxy_http2_module (shared)


[root@localhost conf.modules.d]# httpd -D DUMP_INCLUDES
Included configuration files:
  (*) /etc/httpd/conf/httpd.conf
    (59) /etc/httpd/conf.modules.d/00-base.conf
    (59) /etc/httpd/conf.modules.d/00-dav.conf
    (59) /etc/httpd/conf.modules.d/00-lua.conf
    (59) /etc/httpd/conf.modules.d/00-mpm.conf
    (59) /etc/httpd/conf.modules.d/00-optional.conf
    (59) /etc/httpd/conf.modules.d/00-proxy.conf
    (59) /etc/httpd/conf.modules.d/00-systemd.conf
    (59) /etc/httpd/conf.modules.d/01-cgi.conf
    (59) /etc/httpd/conf.modules.d/10-h2.conf
    (59) /etc/httpd/conf.modules.d/10-proxy_h2.conf
    (356) /etc/httpd/conf.d/autoindex.conf
    (356) /etc/httpd/conf.d/userdir.conf
    (356) /etc/httpd/conf.d/welcome.conf
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using localhost.localdomain. Set the 'ServerName' directive globally to suppress this message

php 설치에 필요한 모듈 설치
--------------------------------------------------
https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/l/libmcrypt-2.5.8-13.el7.x86_64.rpm

https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/l/libmcrypt-devel-2.5.8-13.el7.x86_64.rpm

yum localinstall -y libmcrypt-2.5.8-13.el7.x86_64.rpm  libmcrypt-devel-2.5.8-13.el7.x86_64.rpm

sudo yum -y install gcc-c++ libxml2-devel openssl-devel bzip2-devel libcurl-devel curl-devel libpng-devel libjpeg-devel freetype-devel t1lib-devel gmp-devel  libicu-devel openldap-devel libmcrypt-devel mysql-devel readline-devel net-snmp-devel libxslt-devel libXpm-devel libevent libevent-devel httpd-devel autoconf libmemcached libmemcached-devel


php 5.5 최신 버전 다운로드
wget https://www.php.net/distributions/php-5.5.38.tar.gz

php 압축해제
tar -xzvf php-5.5.38.tar.gz

cd php-5.5.38

./configure --help

## 추가 설정
sudo make clean
make distclean

-------------------------------------------------
# ZTS 모듈 사용
./configure --enable-sigchild --with-apxs2=/usr/bin/apxs --with-config-file-path=/opt/apache/conf --with-config-file-scan-dir=/etc/php-zts-5.5.d --with-kerberos --with-openssl=shared --with-zlib=shared --enable-bcmath=shared --with-bz2=shared --with-curl=shared --enable-ctype=shared --enable-dom=shared --enable-calendar=shared --enable-exif=shared --enable-fileinfo=shared --enable-ftp=shared --with-gd=shared --with-iconv=shared --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext=shared --with-gmp=shared --with-mhash --enable-intl=shared --with-ldap=shared --enable-mbstring=shared --with-onig --enable-json=shared --with-mcrypt=shared --with-mysql=shared --enable-pcntl --enable-pdo=shared --with-pdo-mysql=shared --enable-mysqlnd=shared --with-pdo-sqlite=shared  --enable-phar=shared --enable-posix=shared --with-mysqli=shared --with-readline --enable-shmop=shared --enable-simplexml=shared --enable-sockets=shared --with-sqlite3=shared --with-snmp --enable-opcache --enable-soap=shared --enable-sysvmsg=shared --enable-sysvsem=shared --enable-sysvshm=shared --enable-tokenizer=shared --enable-wddx=shared --with-xsl=shared --enable-zip=shared --enable-zend-signals --with-freetype-dir --with-t1lib --with-xpm-dir --enable-xml=shared --with-xmlrpc=shared --enable-xmlreader=shared --enable-xmlwriter=shared --with-libdir=lib64 --enable-fpm --with-tsrm-pthreads --enable-maintainer-zts --prefix=/usr/local/php-zts

make && sudo make install


https://louet.tistory.com/124


-------------------------------------------------------

phpredis 설치

unzip phpredis-2.2.8.zip

cd phpredis-2.2.8

sudo find / -name phpize
sudo find / -name php-config

/usr/local/php-zts/bin/phpize

./configure --with-php-config=/usr/local/php-zts/bin/php-config

make && sudo make install

ls -la  /usr/local/php-zts/lib/php/extensions/no-debug-zts-20121212/

---------------------------------------------
# php-memcached 설치

https://github.com/php-memcached-dev/php-memcached/tree/2.2.0

unzip php-memcached-2.2.0.zip

cd php-memcached-2.2.0
/usr/local/php-zts/bin/phpize
./configure --with-php-config=/usr/local/php-zts/bin/php-config
make && sudo make install

ls -la  /usr/local/php-zts/lib/php/extensions/no-debug-zts-20121212/

memcached.so 확인


---------------------------------------------
# igbinary 설치

cd ..

https://github.com/igbinary/igbinary/tree/2.0.8

unzip igbinary-2.0.8.zip

cd igbinary-2.0.8

sudo make clean

make distclean

/usr/local/php-zts/bin/phpize
./configure --with-php-config=/usr/local/php-zts/bin/php-config
make && sudo make install

ls -la  /usr/local/php-zts/lib/php/extensions/no-debug-zts-20121212/

