#!/bin/bash
mkdir /var/www/
sudo mount -t efs -o tls,accesspoint=fsap-0336048fbb5688cc2 fs-08618216c6f97b55e:/ /var/www/
yum install -y httpd 
systemctl start httpd
systemctl enable httpd
yum module reset php -y
yum module enable php:remi-7.4 -y
yum install -y php php-common php-mbstring php-opcache php-inqtl php-xml php-gd php-curl php-mysqlnd php-fpm php-json
systemctl start php-fpm
systemctl enable php-fpm
git clone https://github.com/nbomasi/tooling.git
mkdir /var/www/html
cp -R /tooling/html/*  /var/www/html/
cd /tooling
mysql -h smile-rsd-db.cq3vb6n1ikef.us-east-1.rds.amazonaws.com -u admin -p toolingdb < tooling-db.sql
cd /var/www/html/
touch healthstatus
sed -i "s/$db = mysqli_connect('mysql.tooling.svc.cluster.local', 'admin', 'admin', 'tooling');/$db = mysqli_connect('smile-rsd-db.cq3vb6n1ikef.us-east-1.rds.amazonaws.com', 'admin', 'admin12345', 'toolingdb');/g" functions.php
chcon -t httpd_sys_rw_content_t /var/www/html/ -R
systemctl restart httpd







