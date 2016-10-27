## Raspberry Pi 3 First Install and Web Server

Raspberry Pi | Web Server | Database | PHP | Node | Monitor
---|---|---|---|---|---
#3 Model B | Nginx | MongoDB and MySQL  | 7.0 | 0.0 | RPi-Monitor

## Step 1.0: Getting Started

Format The SD Card with `SDFormatter`
- [SDFormatter](https://www.sdcard.org/downloads/formatter_4/index.html)

Getting the SD Card to install `Raspbian Jessie Lite` in terminal
1. View your drive directory location
```
$ diskutil list
```

2. Unmount the SD Card
```
$ diskutil unmountDisk /dev/disk2
```

3. Install the Linux to the SD Card
```
$ sudo dd bs=1m if=2016-09-23-raspbian-jessie-lite.img of=/dev/disk2
```

4. Eject the SD Card
```
$ sudo diskutil eject /dev/disk2
```

## Step 1.1: Inside the Raspberry Pi 3

Log in
- Login: pi
- Password: raspberry

Raspbian Configuration
```
$ sudo raspi-config
```
- SSH server enabled
- Boot Option
- Hostname

## Step 1.2: In the Terminal (Mac)
SSH to the pi
```
$ ssh pi@192.168.0.25
```

Clear your SSH Key if you need to, use your IP address
```
$ ssh-keygen -R 192.168.0.25
```

## Step 1.3: Inside the Raspberry Pi 3
Raspbian Updates
```
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt-get upgrade
```

- Install Tree (directory viewer)
```
$ sudo apt-get install tree
```

## Step 1.4: Installing PHP 7.0 and Nginx
Change into the testing branch of `Raspbian`, codename `stretch`
```
$ sudo nano /etc/apt/sources.list
```

Edit the `sources.list` to
```
deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://archive.raspbian.org/raspbian/ jessie main contrib non-free rpi
```

Pin all packages to use the `jessie` release with a higher priority by default
```
$ sudo nano /etc/apt/preferences
```

Past to `preferences`
```
Package: *
Pin: release n=jessie
Pin-Priority: 600
```

Raspbian Updates
```
$ sudo apt-get update
```

Install `PHP 7.0` from the stretch release including all the common PHP packages
```
$ sudo apt-get install -t stretch php7.0 php7.0-curl php7.0-gd php7.0-fpm php7.0-cli php7.0-opcache php7.0-mbstring php7.0-xml php7.0-zip
```

Check the PHP version
```
$ php -v
```

Install `nginx`
```
$ sudo apt-get install -t stretch nginx
```

Check the nginx version
```
$ nginx -v
```

Search for vulnerability or exploit for nginx `version #`
- Patch it and move on for now.


View Nginx in your browser
```
http://192.168.0.25/
```

Go to directory
```
cd /var/www/html
```

Make a new file `info.php`
```
$ sudo nano info.php
```

Past to `info.php`
```
<?php

// Show all information, defaults to INFO_ALL
phpinfo();

// Show just the module information.
// phpinfo(8) yields identical results.
phpinfo(INFO_MODULES);

?>
```

Edit PHP 7.0 FPM pool to use our pi user and group
```
$ sudo nano /etc/php/7.0/fpm/pool.d/www.conf
```

Pasta to `www.conf`
```
user = pi
group = pi
```

Create a new `nginx configuration` of the name of your domain
```
$ sudo nano /etc/nginx/sites-available/YourDomainName
```

Past to `YourDomainName`
```
server {
    #listen 80;
    index index.html index.php;

    ## Begin - Server Info
    root /var/www/hellocode;
    server_name hellocode;
    ## End - Server Info

    ## Begin - Index
    # for subfolders, simply adjust:
    # `location /subfolder {`
    # and the rewrite to use `/subfolder/index.php`
    location / {
        try_files $uri $uri/ /index.html /index.php;
    }
    ## End - Index

    ## Begin - PHP
    location ~ \.php$ {
        # Choose either a socket or TCP/IP address
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        # fastcgi_pass 127.0.0.1:9000;

        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
    }
    ## End - PHP

    ## Begin - Security
    # deny all direct access for these folders
    location ~* /(.git|cache|bin|logs|backups|tests)/.*$ { return 403; }
    # deny running scripts inside core system folders
    location ~* /(system|vendor)/.*\.(txt|xml|md|html|yaml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
    # deny running scripts inside user folder
    location ~* /user/.*\.(txt|md|yaml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
    # deny access to specific files in the root folder
    location ~ /(LICENSE.txt|composer.lock|composer.json|nginx.conf|web.config|htaccess.txt|\.htaccess) { return 403; }
    ## End - Security
}
```

Change the `default` site not to load but with a new load of the new `YourDomainName` site instead

CD directory
```
$ cd /etc/nginx/sites-enabled/
```

Remove `default`
```
$ sudo rm default
```

Add the new domain `YourDomainName`
```
$ ln -s /etc/nginx/sites-available/YourDomainName
```

Create a new directory for `YourDomainName`
```
$ cd /var/www/
$ sudo mkdir cardboard
$ cd YourDomainName
```

Make a home file.
```
$ sudo nano index.html
```

PHP info file.
```
$ sudo nano info.php
```

paste to `info.php`
```
<?php

// Show all information, defaults to INFO_ALL
phpinfo();

// Show just the module information.
// phpinfo(8) yields identical results.
phpinfo(INFO_MODULES);

?>
```

Restart PHP and nginx
```
$ sudo /etc/init.d/nginx restart
$ sudo /etc/init.d/php7.0-fpm restart
```



Add a new site
```
$ cd /etc/nginx/sites-available
$ sudo cp default juliomontas
```

Lets Enable the site
```
$ sudo ln -s /etc/nginx/sites-available/juliomontas /etc/nginx/sites-enabled/juliomontas
```

Make the website folder
```
$ cd var/www && mkdir juliomontas && touch index.html
```

Debug in Nginx
```
$ sudo service nginx force-reload
$ sudo service nginx status
$ sudo service nginx configtest
$ sudo nginx -t
```




sudo nano /etc/nginx/sites-available/
sudo chown -R www-data:www-data var/www/practicaldoctor
cd /etc/nginx/sites-available
sudo cp default practicaldoctor
sudo ln -s /etc/nginx/sites-available/practicaldoctor /etc/nginx/sites-enabled/practicaldoctor
sudo /etc/init.d/nginx reload
sudo nano /etc/nginx/sites-available/practicaldoctor
root /var/www/practicaldoctor

sudo ln -s /etc/nginx/sites-available/practicaldoctor /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/cardboard /etc/nginx/sites-enabled/








Installing MySQL and set a password for root user
```
$ sudo apt-get mysql-server
$ sudo apt-get remove php7.0-fpm php7.0-curl php7.0-gd php7.0-cli php7.0-mcrypt php7.0-mysql php-apc
```

Securing MySQL
```
$ sudo mysql_secure_installation
```


































## Step 1.4: Installing of Nginx
```
sudo apt-get install nginx
```

View Nginx in your browser
```
http://192.168.0.25/
```

## Step 1.5: Install RPi-Monitor
Install Git
```
$ sudo apt-get install git
```

Get the latest Packages
```
$ git clone https://github.com/XavierBerger/RPi-Monitor-deb.git
```

Enter the `packages` directory
```
$ cd RPi-Monitor-deb/packages
```

Enter the `packages` directory
```
$ sudo apt-get install librrds-perl libhttp-daemon-perl libwww-perl libjson-perl libipc-sharelite-perl libfile-which-perl
```

Force the installation
```
$ sudo apt-get -f install
```

Install RPi-Monitor
```
$ sudo dpkg -i rpimonitor_2.9.1-1_all.deb
```

Permission and update for RPi-Monitor
```
$ sudo apt-get update && sudo /usr/share/rpimonitor/scripts/updatePackagesStatus.pl
```

View RPi-Monitor in your browser
```
http://192.168.0.25:8888/status.html
```

If you having problems try this commands
```
$ sudo apt-get update && sudo service rpimonitor update
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo reboot
```

RPi-Monitor Option
```
$ sudo /etc/init.d/rpimonitor {start|stop|restart|status}
```

<!-- Shell In A Box Option
```
$ sudo /etc/init.d/shellinabox {start|stop|restart|force-reload|reload}
``` -->

Check the active internet connections
```
$ sudo netstat -nap
```















CD to the `www` directory
```
cd /var/www/html
```

Make a php file
```
sudo touch info.php
```

Edit the `info.php`
```
sudo nano info.php
```

Enter inside `info.php`
```php
sudo nano info.php
```



























## Step 1.1: Setup your Router

Google `{Router Name} router LAN IP` to get the default IP
Google `{Router Name} router password` to get the default Username and Password



Install Later
- [Piwik (Analytics)](https://piwik.org/docs/installation/#the-5-minut-piwik-installation)


Reference Links
- [Raspberry Pi Dev Setup with Nginx + PHP7](https://getgrav.org/blog/raspberrypi-nginx-php7-dev)
- [Creating a Web Server on a Raspberry Pi](http://www.jeffbondono.com/WebSiteRaspPi.html)
- [Install MongoDB and Node.js on a Raspberry Pi](http://www.jeffbondono.com/WebSiteRaspPi.html)
- [Installing Node.js on a Raspberry Pi 3](https://blog.wia.io/installing-node-js-on-a-raspberry-pi-3)
- [How to set up a secure Raspberry Pi web server, mail server and Owncloud installation](https://pestmeester.nl/)
- [MongoDB 3.0.9 binaries for Raspberry Pi 2 & 3 (Jessie)](http://andyfelong.com/2016/01/mongodb-3-0-9-binaries-for-raspberry-pi-2-jessie/)
- [How to transfer a domain from Bluehost](https://www.namecheap.com/support/knowledgebase/article.aspx/1207/83/how-to-transfer-a-domain-from-bluehost)

- [Shell In A Box – A Web-Based SSH Terminal to Access Remote Linux Servers](Shell In A Box – A Web-Based SSH Terminal to Access Remote Linux Servers)


- [20 Command Line Tools to Monitor Linux Performance](http://www.tecmint.com/command-line-tools-to-monitor-linux-performance/)
