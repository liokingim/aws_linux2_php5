subscription-manager register --username <username> --password <password> --auto-attach

데스크톱

서브 스크립션 등록

sudo yum update

sudo yum install gnome-tweaks -y

sudo yum install gnome-extensions-app -y

sudo yum install gnome-shell-extensions


# 확장 선택

Applications Menu ON
Window List ON

유틸리티 > 기능개선

창제목 표시줄
제목 표시줄 단추 최대화 ON, 최소화 ON

최상위 표시줄
요일 표시

# 프로그램 > 기타 설정

언어 설정

정보
 서브 스크립션 정보 표시


sudo systemctl start cockpit

10.0.2.15:9090

웹브라우저에서 ifconfig 아이피 확인


웹 브라우저에서 아이피:9090으로 접속

관리자로 엑세스 가능

-------------------------------------------------------------------

apache tomcat

sudo yum list |grep tomcat
sudo yum list |grep java
sudo yum list |grep mysql

Tomcat 9

sudo useradd tomcat

sudo id tomcat

sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.83/bin/apache-tomcat-9.0.83.tar.gz


tar zxvf apache-tomcat-9.0.83.tar.gz

sudo mv apache-tomcat-9.0.83 /opt/

sudo chown -R tomcat. /opt/apache-tomcat-9.0.83/

sudo su - tomcat
cd /opt/apache-tomcat-9.0.83/bin/
./startup.sh

root > jps

/opt/apache-tomcat-9.0.83/bin/shutdown.sh

/opt/apache-tomcat-9.0.83/bin/startup.sh

/opt/apache-tomcat-9.0.83/bin/version.sh

# 유저 권한 설정
sudo vi /opt/apache-tomcat-9.0.83/conf/tomcat-users.xml

<role rolename="admin-gui"/>
<user username="tomcat" password="password" roles="admin-gui"/>

/opt/apache-tomcat-9.0.83/bin/shutdown.sh
/opt/apache-tomcat-9.0.83/bin/startup.sh

http://localhost:8080/host-manager/html 는 로컬에서만 확인가능.

less /opt/apache-tomcat-9.0.83/logs/catalina.out


find / -name context.xml


외부에서 볼수 있도록 할려면 아래를 수정


sudo vi /opt/apache-tomcat-9.0.83/webapps/host-manager/META-INF/context.xml

 <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
아이피 허용부분을 삭제


<role rolename="admin-gui"/>
<role rolename="manager-gui"/>
<user username="admin" password="password" fullName="Administrator" roles="admin-gui,manager-gui"/>

sudo vi /opt/apache-tomcat-9.0.83/conf/server.xml

netstat -anop |grep 8080

설치 되어 있지 않다면,
yum provides netstat

yum install net-tools-2.0-0.51.20160912git.el8.x86_64

# ファイアウォールを構成 외부 접속 허용
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=8443/tcp
sudo firewall-cmd --reload


httpd -D DUMP_MODULES


https://ja.linux-console.net/?p=21623#google_vignette





Apache Tomcat アプリケーション コンテナへのプロキシとして Apache httpd を使用します。以下のコマンドを使用して httpd パッケージをインストールします。
sudo yum -y install httpd


sudo vi /etc/httpd/conf.d/tomcat_manager.conf

<VirtualHost *:80>
    ServerAdmin root@localhost
    ServerName tomcat.example.com
    DefaultType text/html
    ProxyRequests off
    ProxyPreserveHost On
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
</VirtualHost>


sudo systemctl restart httpd && sudo systemctl enable httpd

-------------------------------------------------------------------

## java

sudo yum list installed "java*"

# 이전 버전 삭제
sudo yum remove java-1.8.0-openjdk.x86_64
sudo yum remove java-1.8.0-openjdk-headless.x86_64

sudo wget https://javadl.oracle.com/webapps/download/AutoDL?BundleId=249192_b291ca3e0c8548b5a51d5a5f50063037


sudo yum install java-17-openjdk.x86_64

java -version


sudo yum module install mysql:8.0/server


手順

mysql モジュールから 8.0 ストリーム (バージョン) を選択し、server プロファイルを指定して、MySQL サーバーパッケージをインストールします。

# yum module install mysql:8.0/server
mysqld サービスを開始します。

# sudo systemctl start mysqld.service
mysqld サービスを有効にして、起動時に起動するようにします。

# sudo systemctl enable mysqld.service
推奨手順: MySQL のインストール時にセキュリティーを強化するには、次のコマンドを実行します。

$ sudo mysql_secure_installation
このコマンドは、完全にインタラクティブなスクリプトを起動して、プロセスの各ステップのプロンプトを表示します。このスクリプトを使用すると、次の方法でセキュリティーを改善できます。

root アカウントのパスワードの設定
匿名ユーザーの削除
リモート root ログインの拒否 (ローカルホスト外)




-------------------------------------------------------------------

yum install java-1.8.0-openjdk
systemctl enable tomcat8.service
systemctl start tomcat8
systemctl status tomcat8
systemctl stop tomcat8
systemctl restart tomcat8

Systemd File:

Systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/usr/local/tomcat8/temp/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat8
Environment=CATALINA_BASE=/usr/local/tomcat8
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/usr/local/tomcat8/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat8
Group=tomcat8
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

--------------------------------------------------------------------------

# ssl 작성 자체 서명 인증서
find / -name keytool

cd /usr/lib/jvm/java-17-openjdk-17.0.9.0.9-2.el8.x86_64/bin

keytool -genkey -keysize 2048 -keyalg RSA -noprompt -alias tomcat -dname "CN=localhost, OU=NA, O=NA, L=NA, S=NA, C=NA" -keystore keystore.jks -validity 9999 -storepass changeme -keypass changeme

keytool -certreq -alias yourdomain -keystore keystore.jks > keycloak.careq

keytool -v -list -keystore keystore.jks

changeme

서명 확인

톰캣 conf 밑으로 복사

cp keystore.jks /opt/apache-tomcat-9.0.83/conf/

sudo vi /opt/apache-tomcat-9.0.83/conf/server.xml

주석 해제

    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true"
               maxParameterCount="1000"
               >
        <SSLHostConfig>
            <Certificate certificateKeyAlias="tomcat"
                certificateKeystorePassword="changeme"
                certificateKeystoreFile="conf/keystore.jks"
                type="RSA" />
        </SSLHostConfig>
    </Connector>


/opt/apache-tomcat-9.0.83/bin/shutdown.sh
/opt/apache-tomcat-9.0.83/bin/startup.sh

ps aux |grep tomcat

cat /opt/apache-tomcat-9.0.83/logs/catalina.out

https://localhost:8443/

------------------------------------------------------------------------

[root@localhost bin]# ls -Z startup.sh
unconfined_u:object_r:user_home_t:s0 startup.sh
[root@localhost bin]# ls -Z shutdown.sh
unconfined_u:object_r:user_home_t:s0 shutdown.sh
[root@localhost bin]# chcon -t httpd_sys_script_exec_t startup.sh
[root@localhost bin]# chcon -t httpd_sys_script_exec_t shutdown.sh

chcon -t user_home_t startup.sh
chcon -t user_home_t shutdown.sh


------------------------------------------------------------------------
# SELinux 컨텍스트 정책 변경

ausearch -c 'startup.sh' --raw | audit2allow -M my-startupsh
    Nothing to do


semodule -X 300 -i my-startupsh.pp


tail /var/log/audit/audit.log

grep 1702447867.140:297 /var/log/audit/audit.log | audit2allow -M my-startupsh

생성 확인
my-startupsh.pp
my-startupsh.te

semodule -i my-startupsh.pp

/sbin/restorecon -v /opt/apache-tomcat-9.0.83/bin/startup.sh

systemctl start tomcat.service


# SELinux 파일 컨텍스트를 설정 httpd

sudo semanage fcontext -a -t httpd_log_t '/data/httpd/log/error_log'
sudo restorecon -v '/data/httpd/log/error_log'
sudo semanage fcontext -a -t httpd_log_t '/data/httpd/log/access_log'
sudo restorecon -v '/data/httpd/log/access_log'
systemctl restart httpd

sudo setsebool -P httpd_can_network_connect 1

# Tomcat setenv.sh

/opt/tomcat/bin/setenv.sh

sudo vi setenv.sh

CATALINA_OPTS="-server -Xmx1024m -Xms1024m -Xss512k"
export CATALINA_OPTS

# LibreOffice
wget https://ftp-srv2.kddilabs.jp/office/tdf/libreoffice/stable/7.6.4/rpm/x86_64/LibreOffice_7.6.4_Linux_x86-64_rpm.tar.gz

tar zxvf LibreOffice_7.6.4_Linux_x86-64_rpm.tar.gz

cd RPMS

yum localinstall *.rpm

rpm -qa |grep libreoffice

rpm -e libreoffice

rpm -e $(rpm -qa | grep -i -E "^libreoffice|libobasis")

## 6.4.7.2
yum install libreoffice.x86_64

yum remove libreoffice.x86_64

wget https://downloadarchive.documentfoundation.org/libreoffice/old/6.4.7.2/rpm/x86_64/LibreOffice_6.4.7.2_Linux_x86-64_rpm.tar.gz

wget https://pkgs.dyn.su/el8/base/x86_64/ipa-gothic-fonts-003.03-15.el8.noarch.rpm

wget https://pkgs.dyn.su/el8/base/x86_64/ipa-pgothic-fonts-003.03-14.el8.noarch.rpm

wget http://mirror.centos.org/altarch/7/os/aarch64/Packages/ipa-mincho-fonts-003.03-5.el7.noarch.rpm

wget http://mirror.centos.org/centos/7/os/x86_64/Packages/ipa-pmincho-fonts-003.03-5.el7.noarch.rpm

rpm -Uvh ipa-gothic-fonts-003.03-15.el8.noarch.rpm
rpm -Uvh ipa-mincho-fonts-003.03-5.el7.noarch.rpm
rpm -Uvh ipa-pgothic-fonts-003.03-14.el8.noarch.rpm
rpm -Uvh ipa-pmincho-fonts-003.03-5.el7.noarch.rpm

# LibreOffice 의존 라이브러리




