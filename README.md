## nginx pi

* https://mattwilcox.net/web-development/setting-up-a-secure-home-web-server-with-raspberry-pi/
* https://mattwilcox.net/web-development/setting-up-a-secure-website-with-https-and-spdy-support-under-nginx-on-a-raspberry-pi

### after init setup

* updates

`sudo apt-get update && sudo apt-get upgrade -y`

* add user/s to all present `groups` to disable `pi` login

`sudo useradd -m -G adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,netdev,input USERNAME`

`sudo passwd USERNAME`

* usb? find and format for linux

`sudo fdisk -l`

`sudo mkfs.ext4 /dev/sda1` - not /dev/mmcblk0

mountpoint: `sudo mkdir /websites`

`sudo mount /dev/sda1 /websites`

group: `sudo chgrp -R users`

`sudo chmod -R g+w /websites`

automount: `sudo vim /etc/fstab`

=> append `/dev/sda1 [tab] /websites [tab] ext4 [tab] defaults [tab] 0 [tab] 2`

### ssh

```
ssh-keygen -b 4096
...
ssh-copy-id example_user@000.111.333.222
```

**/etc/ssh/sshd_config** PermitRootLogin no, PasswordAuthentication no

`sudo service ssh restart` (probably as service here?)

### firewall

apf

* http://snipe.net/2011/10/apf-bfd-firewall-brute-force/

and fail2ban `sudo apt-get install fail2ban`

### nginx + faster php packs

this installs old nginx version. for security better build from src - https://mattwilcox.net/web-development/setting-up-a-secure-website-with-https-and-spdy-support-under-nginx-on-a-raspberry-pi

`sudo apt-get install nginx php5-fpm php5-curl php5-gd php5-cli php5-mcrypt php5-mysql php-apc`

conf: 

* `sudo vim /etc/nginx/nginx.conf` worker_processes not 4 but 2
* in http block un-comment the 'server_tokens off' line
* in http block against ddos:

```
client_header_timeout 10;
client_body_timeout   10;
keepalive_timeout     10 10;
send_timeout          10;
```

gzip block:

```
gzip on;
gzip_disable "msie6";

gzip_min_length   1100;
gzip_vary         on;
gzip_proxied      any;
gzip_buffers      16 8k;
gzip_comp_level   6;
gzip_http_version 1.1;
gzip_types        text/plain text/css applciation/json application/x-javascript text/xml application/xml application/rss+xml text/javascript images/svg+xml application/x-font-ttf font/opentype application/vnd.ms-fontobject;
```

### opt nginx for php

`sudo vim /etc/nginx/fastcgi_params`

http://wiki.nginx.org/PHPFcgiExample

```
fastcgi_param   QUERY_STRING            $query_string;
fastcgi_param   REQUEST_METHOD          $request_method;
fastcgi_param   CONTENT_TYPE            $content_type;
fastcgi_param   CONTENT_LENGTH          $content_length;

fastcgi_param   SCRIPT_FILENAME         $document_root$fastcgi_script_name;
fastcgi_param   SCRIPT_NAME             $fastcgi_script_name;
fastcgi_param   PATH_INFO               $fastcgi_path_info;
fastcgi_param   REQUEST_URI             $request_uri;
fastcgi_param   DOCUMENT_URI            $document_uri;
fastcgi_param   DOCUMENT_ROOT           $document_root;
fastcgi_param   SERVER_PROTOCOL         $server_protocol;

fastcgi_param   GATEWAY_INTERFACE       CGI/1.1;
fastcgi_param   SERVER_SOFTWARE         nginx/$nginx_version;

fastcgi_param   REMOTE_ADDR             $remote_addr;
fastcgi_param   REMOTE_PORT             $remote_port;
fastcgi_param   SERVER_ADDR             $server_addr;
fastcgi_param   SERVER_PORT             $server_port;
fastcgi_param   SERVER_NAME             $server_name;

fastcgi_param   HTTPS                   $https;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param   REDIRECT_STATUS         200;
```

and in php conf

`sudo vim /etc/php5/fpm/pool.d/www.conf` uncomment listen.owner and listen.group lines


### mysql

`sudo apt-get install mysql-server` ... set passwd

`sudo mysql_install_db` ...setup directory layout

`sudo mysql_secure_installation` ...all y

conf: `sudo vim /etc/mysql/my.cnf`

to check: 

* bind-address set to loopback device 127.0.0.1 (no external connections)
* add to same section (mysqld): `local-infile=0`
* same section - logging: `log=/var/log/mysql-logfile` while /var/log/mysql* not world-readable



## general nginx hardening

* http://www.acunetix.com/blog/articles/nginx-server-security-hardening-configuration-1/
* http://www.tecmint.com/nginx-web-server-security-hardening-and-performance-tips/

## irc server

* https://www.digitalocean.com/community/tutorials/how-to-set-up-an-irc-server-on-ubuntu-14-04-with-inspircd-2-0-and-shalture

## hidden

* http://www.makeuseof.com/tag/create-hidden-service-tor-site-set-anonymous-website-server/
