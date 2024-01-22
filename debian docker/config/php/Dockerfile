FROM php:5.6.40-apache

COPY sources.list /etc/apt/

RUN <<EOF
apt-get update
apt-get install -y openssh-server
apt-get install -y vim
EOF

CMD ["service", "ssh", "start"]
