wget https://www.php.net/distributions/php-5.5.38.tar.gz

tar -xzvf php-5.5.38.tar.gz

sudo yum install -y gcc make libxml2-devel openssl-devel bzip2-devel libcurl-devel libjpeg-devel libpng-devel libicu-devel libmcrypt-devel libxslt-devel httpd-devel

cd php-5.5.38

./configure --with-apxs2 --with-mysql --with-mysqli --with-pdo-mysql --with-bz2 --with-curl --with-gd --with-jpeg-dir=/usr --with-png-dir=/usr --with-openssl --with-zlib --with-gettext --enable-mbstring --enable-fpm --enable-pcntl --enable-opcache --enable-soap --enable-sockets --enable-zip

make && sudo make install

sudo cp php.ini-development /usr/local/lib/php.ini
sudo vi /usr/local/lib/php.ini

extension=mysqli.so
extension=pdo_mysql.so

sudo cp sapi/fpm/php-fpm.conf /usr/local/etc/
sudo cp sapi/fpm/php-fpm.service /usr/lib/systemd/system/
sudo systemctl enable php-fpm
sudo systemctl start php-fpm

sudo vi /etc/httpd/conf/httpd.conf

LoadModule php5_module        modules/libphp5.so
AddType application/x-httpd-php php

sudo systemctl restart httpd

