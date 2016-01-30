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

`sudo mkdir /mountpoint`

`sudo mount /dev/sda1 /mountpoint`

group: `sudo chgrp -R users`

`sudo chmod -R g+w /mountpoint`

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

`sudo apt-get install nginx php5-fpm php5-curl php5-gd php5-cli php5-mcrypt php5-mysql php-apc`

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
