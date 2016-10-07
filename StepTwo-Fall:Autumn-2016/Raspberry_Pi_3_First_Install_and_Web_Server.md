## Raspberry Pi 3 First Install and Web Server

Raspberry Pi | Web Server | Database | PHP | Node
---|---|---|---|---
#3 Model B | Nginx | MongoDB and MySQL  | 7.0 | 0.0

## Step I: Getting Started

#### Format The SD Card with `SDFormatter`
- [SDFormatter](https://www.sdcard.org/downloads/formatter_4/index.html)

#### Getting the SD Card to install `Raspbian Jessie Lite` in terminal
1. View your drive directory location
```
$ diskutil list
```

- Unmount the SD Card
```
$ diskutil unmountDisk /dev/disk2
```

- Install the Linux to the SD Card
```
$ sudo dd bs=1m if=2016-09-23-raspbian-jessie-lite.img of=/dev/disk2
```

- Eject the SD Card
```
$ sudo diskutil eject /dev/disk2
```

- Clear your SSH Key if you need to, use your IP address
```
$ ssh-keygen -R 192.168.0.0
```


## Step II: Inside the Raspberry Pi 3

#### First Install

- Updating Firmware
```
$ sudo rpi-update
$ sudo reboot
```

- Raspbian Updates
```
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt-get upgrade
```

- Raspbian Configuration
```
$ sudo raspi-config
```

#### Global Install
- Git (version control system)
```
$ sudo apt-get install git-all
```

- Tree (directory viewer)
```
$ sudo apt-get install tree
```

#### Installing PHP 7
- Change into the testing branch of `Raspbian`, codename `stretch`
```
$ sudo nano /etc/apt/sources.list
```

- Edit the `sources.list` to
```
deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://archive.raspbian.org/raspbian/ jessie main contrib non-free rpi
```

- Pin all packages to use the `jessie` release with a higher priority by default
```
$ sudo nano /etc/apt/preferences
```

- Edit `preferences` to
```
Package: *
Pin: release n=jessie
Pin-Priority: 600
```

- Raspbian Updates
```
$ sudo apt-get update
```

- Install `PHP 7.0` from the stretch release including all the common PHP packages
```
$ sudo apt-get install -t stretch php7.0 php7.0-curl php7.0-gd php7.0-fpm php7.0-cli php7.0-opcache php7.0-mbstring php7.0-xml php7.0-zip
```

#### Installing of Nginx
```
sudo apt-get install -t stretch nginx
```

STOPPPPPP



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
