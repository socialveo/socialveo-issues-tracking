![Socialveo Issues Tracking](https://socialveo.com/wp-content/themes/socialveo/images/logo/socialveo-normal.png)

# Socialveo Release

# Installation script

### Linux ✔

1. Install `php5.6`

2. Run installation script
```sh
./install-linux.sh
```

# Hand installation

## Linux

### Install php5.6

#### [Debian-based linux](https://www.debian.org/misc/children-distros)

Install custom php repo

```
sudo add-apt-repository ppa:ondrej/php
```

```bash
sudo apt-get update
sudo apt-get install php5-cli php5-fpm
```

#### [Rpm-based linux](https://en.wikipedia.org/wiki/Category:RPM-based_Linux_distributions)

```bash
sudo yum update
sudo yum install php5-cli php5-fpm
```

Check your installed php version:
To verify if PHP is installed already, enter `php -v`.
If PHP is installed, messages similar to the following display:

```
PHP 5.6.4 (cli) (built: Dec 20 2014 17:30:46)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2014 Zend Technologies
with Zend OPcache v7.0.4-dev, Copyright (c) 1999-2014, by Zend Technologies
```

In some system by default installed php version different from `php5.6`,
so you need to remove installed `php` and after install custom `php` repositories install `php5.6`.

#### Links

* [Install PHP 5.6 on Debian 7 (Wheezy)](https://www.devops.zone/tricks/install-php-5-6-on-debian-wheezy/)
* [Setup NGINX, PHP-FPM, and MariaDB on Debian 8 (Jessie)](https://www.vultr.com/docs/setup-up-nginx-php-fpm-and-mariadb-on-debian-8)
* [Install php 5.6 on Ubuntu](http://askubuntu.com/questions/756181/installing-php-5-6-on-xenial-16-04)
* [PHP 5.6 on CentOS 6 or 7](http://devdocs.magento.com/guides/v2.0/install-gde/prereq/php-centos.html#instgde-prereq-php56-install-centos)
* [PHP 5.6 on CentOS/RedHat 7.2 and 6.8 via Yum](https://webtatic.com/packages/php56/)

### Install required php packages
 
#### [Debian-based linux](https://www.debian.org/misc/children-distros)

```sh
sudo apt-get install php5.6-cli php5.6-opcache php5.6-fpm php5.6-common php5.6-curl php5.6-bcmath php5.6-gd php5.6-json php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-readline php5.6-xml php5.6-pdo php5.6-intl php5.6-zip php5.6-uploadprogress php5.6-zip php5.6-intl

```

#### [Rpm-based linux](https://en.wikipedia.org/wiki/Category:RPM-based_Linux_distributions)

```sh
yum install php5-cli php5-opcache php5-fpm php5-common php5-bcmath php5-curl php5-gd php5-json php5-mbstring php5-mcrypt php5-mysql php5-readline php5-xml php5-pdo php5-zip php5-intl
```

#### Additional packages:

```
php5-igbinary
php5-imagick
php5-memcache
php5-memcached
php5-msgpack
```

### Install Socialveo

Open terminal, go to path where located `socialveo-release` package:
```sh
cd path/to/socialveo-release
```

Create directory in `/var/www` (change `my-site.com` to your domain name)
```sh
mkdir -m 0777 /var/www/my-site.com
```

Copy socialveo files:
```sh
cp -f ./package/. /var/www/my-site.com
```

Restart php-fpm service (depending of your system):
```
systemctl restart php5-fpm
```
or
```
service php5-fpm restart
```
or
```
/etc/init.d/php5-fpm stop
/etc/init.d/php5-fpm start
```

#### Install phalcon & socialveo extensions

First download extensions:
```sh
wget http://socialveo.co/ext/socialveo/1.0.0/socialveo.so
wget http://socialveo.co/ext/phalcon/2.0.12/phalcon.so
```

or for dev-only download from link
https://github.com/socialveo/phalcon/raw/master/build/64bits/modules/phalcon.so


After that you need to know a directory where located php modules, it can be `/usr/lib/php` or `/usr/lib64/php`
Copy phalcon & socialveo extensions (change `php_mod_path` to actual for your OS):
```sh
php_mod_path=/usr/lib/php
if [[ -d "${php_mod_path}/20131226" ]]; then
    php_mod_path="${php_mod_path}/20131226"
fi
cp -f socialveo.so "${php_mod_path}/socialveo.so"
cp -f phalcon.so "${php_mod_path}/phalcon.so"
```

After that you need to know php ini path,
you can get it by running:
```sh
php --ini | grep .ini | grep additional
```

It can be something like `/etc/php/5.6/cli/conf.d`, in this case
you need to create ini files in 2 dirs (in `cli` and `fpm`)
```sh
echo "extension=phalcon.so" > /etc/php/5.6/cli/conf.d/30-phalcon.ini
echo "extension=phalcon.so" > /etc/php/5.6/fpm/conf.d/30-phalcon.ini
echo "extension=socialveo.so" > /etc/php/5.6/cli/conf.d/35-socialveo.ini
echo "extension=socialveo.so" > /etc/php/5.6/fpm/conf.d/35-socialveo.ini
```

or it can be `/etc/php.d`, in this case you need run:
```sh
echo "extension=phalcon.so" > /etc/php.d/zz30-phalcon.ini
echo "extension=socialveo.so" > /etc/php.d/zz35-socialveo.ini
```

### Install nginx

#### [Debian-based linux](https://www.debian.org/misc/children-distros)

```bash
sudo apt-get update
sudo apt-get install nginx
sudo service nginx start
```

#### [Rpm-based linux](https://en.wikipedia.org/wiki/Category:RPM-based_Linux_distributions)

```sh
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

#### Links

* [Official Nginx Repositories](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)
* [Ho To Install Nginx on Debian 8](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-8)
* [Understanding the Nginx Configuration File Structure and Configuration Contexts](https://www.digitalocean.com/community/tutorials/understanding-the-nginx-configuration-file-structure-and-configuration-contexts)
* [How To Install Nginx on Ubuntu 14.04 LTS](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-14-04-lts)
* [How to install and configure NGINX on Fedora](https://www.godaddy.com/garage/tech/config/how-to-install-and-configure-nginx-on-fedora/)
* [Setting Up Nginx with MariaDB and PHP/PHP-FPM on Fedora 24 Server and Workstation](http://www.tecmint.com/install-nginx-with-mariadb-php-php-fpm-on-fedora-24/)
* [How To Install Nginx on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7)
* [How to Install NGINX on CentOS, Redhat and Fedora](http://tecadmin.net/install-nginx-on-centos-rhel-6-and-5/)
* [Installing Nginx From Source](https://www.nginx.com/resources/admin-guide/installing-nginx-open-source/)

### Configure Nginx

Create file `your-domain.conf` with contents:

```
server {
    listen      ${domain}:80;
    server_name ${domain};
    charset     utf-8;

    access_log  ${root_path}/logs/frontend/nginx_access.log;
    error_log   ${root_path}/logs/frontend/nginx_errors.log warn;

    root        ${root_path}/public;
    index       index.php;

    client_max_body_size 30m;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    fastcgi_buffers 8 16k;
    fastcgi_buffer_size 32k;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    gzip on;
    gzip_comp_level 4;
    gzip_min_length 1000;
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_vary on;

    location ~* ^.+\.(zip|tgz|gz|bz2|odt|txt|tar|avi|mp3|wav|ogg|woff|woff2|eot|ott|svg|ttf)\$ {
        expires 0;
        add_header Cache-Control private;
    }

    location ~* ^.+\.(bmp|png|gif|jpg|jpeg|ico|flv|wmf|swf|pdf|doc|rtf|css|js|json|xml|html|htm|tpl|hta|xhtml|map|less|sass|scss|coffee)\$ {
        expires 0;
        add_header Cache-Control public;
    }

    location ~ /data {
        try_files \$uri =404;
        break;
    }

    location / {
        try_files \$uri \$uri/ /index.php?_url=\$uri&\$args;
        expires -1;
        add_header Cache-Control none;
    }

    location ~ \.php {
        expires -1;
        add_header Cache-Control none;

        include fastcgi_params;

        fastcgi_pass ${php_listen};
        fastcgi_index /index.php;

        fastcgi_split_path_info       ^(.+\.php)(/.+)\$;
        fastcgi_param PATH_INFO       \$fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED \$document_root\$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }

    location ~ /\.git {
        deny all;
    }
}

server {
    listen      ${admin-domain}:80;
    server_name ${admin-domain};
    charset     utf-8;

    access_log  ${root_path}/logs/admin/nginx_access.log;
    error_log   ${root_path}/logs/admin/nginx_errors.log warn;

    root        ${root_path}/public;
    index       admin.php;

    client_max_body_size 30m;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    fastcgi_buffers 8 16k;
    fastcgi_buffer_size 32k;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    gzip on;
    gzip_comp_level 4;
    gzip_min_length 1000;
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_vary on;

    location ~* ^.+\.(zip|tgz|gz|bz2|odt|txt|tar|avi|mp3|wav|ogg|woff|woff2|eot|ott|svg|ttf)\$ {
        expires 0;
        add_header Cache-Control private;
    }

    location ~* ^.+\.(bmp|png|gif|jpg|jpeg|ico|flv|wmf|swf|pdf|doc|rtf|css|js|json|xml|html|htm|tpl|hta|xhtml|map|less|sass|scss|coffee)\$ {
        expires 0;
        add_header Cache-Control public;
    }

    location ~ /data {
        try_files \$uri =404;
        break;
    }

    location / {
        try_files \$uri \$uri/ /index.php?_url=\$uri&\$args;
        expires -1;
        add_header Cache-Control none;
    }

    location ~ \.php {
        expires -1;
        add_header Cache-Control none;

        include fastcgi_params;

        fastcgi_pass ${php_listen};
        fastcgi_index /admin.php;

        fastcgi_split_path_info       ^(.+\.php)(/.+)\$;
        fastcgi_param PATH_INFO       \$fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED \$document_root\$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }

    location ~ /\.git {
        deny all;
    }
}
```

Replace:
* `${domain}` with your domain name,
* `${admin-domain}` with your admin domain name,
* `${root_path}`  with  `/var/www/my-site.com`,
* `${php_listen}` with php listen socket or ip (depending of php config file `www.conf`),
it can be something like `127.0.0.1:9991` (ip address) or `/run/php/php5.6-fpm.sock` (socket)
in this case you need to set prefix `unix:` (e.g. `unix:/run/php/php5.6-fpm.sock`)

Restart nginx service (depending of your system):
```
systemctl restart nginx
```
or
```
service nginx restart
```
or
```
/etc/init.d/nginx stop
/etc/init.d/nginx start
```

### Install mysql

#### [Debian-based linux](https://www.debian.org/misc/children-distros)

```sh
sudo apt-get update
sudo apt-get install mysql
```

or
```
sudo apt-get update
sudo apt install mariadb-server
sudo mysql_secure_installation
```

#### [Rpm-based linux](https://en.wikipedia.org/wiki/Category:RPM-based_Linux_distributions)

Installing MySQL/MariaDB 5

```sh
yum install mariadb mariadb-server
systemctl enable mariadb.service
systemctl start mariadb.service
mysql_secure_installation
```

### Import database dump

Run mysql shell:
```sh
mysql -uUSERNAME -p
```

First create database in mysql:
```mysql
CREATE DATABASE `your_db_name` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Import socialveo dump:
```mysql
SET NAMES utf8mb4;
USE `your_db_name`;
SOURCE /path/to/socialveo-release/schema.sql;
exit;
```


### Configure Socialveo

Open config file `/var/www/my-ste.com`, edit all fields with values like `${value}`.


### Trobleshots

#### Amazon servers

Nginx assign requested address
```
nginx: [emerg] bind() to x.x.x.x:80 failed (99: Cannot assign requested address)
```

Need to tell linux to allow processes to bind to the non-local address. Just add the following line into /etc/sysctl.conf file:

```
# allow processes to bind to the non-local address
# (necessary for apache/nginx in Amazon EC2)
net.ipv4.ip_nonlocal_bind = 1
```

and then reload `sysctl.conf` by:

```sh
$ sysctl -p /etc/sysctl.conf
```
which will be fine on reboots.

#### php-fpm security.limit_extensions

```
FastCGI sent in stderr: "Access to the script '*/public' has been denied (see security.limit_extensions)" while reading response header from upstream, ...
```

for fix it

FPM's security.limit_extension setting is used to limit the extensions of the main script it will be allowed to parse. It prevents malicious code from being executed. The default value is simply .php It can be configured in /etc/php5/fpm/pool.d/www.conf

Though your issue like is elsewhere. The first thing I would check is that the cgi.fix_pathinfo setting in your /etc/php5/fpm/php.ini file is set to:
```
cgi.fix_pathinfo=0
```

#### Languages

For each language pack need to install OS package
To get supported locales:
```sh
locale -a
```

add the locales (for example ru):
```sh
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
```
To update locale
```sh
sudo update-locale
```
And restart php-fpm
```sh
sudo service php5.6-fpm restart
```

