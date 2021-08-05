## Raspberry Pi 3 First Install of Kali Linux

## Step 1.0: Getting Started

Format The SD Card with [SDFormatter](https://www.sdcard.org/downloads/formatter_4/index.html)

View your drive directory location
```
$ diskutil list
```

Unmount the SD Card
```
$ diskutil unmountDisk /dev/disk2
```

Install the Linux to the SD Card
```
$ sudo dd bs=1m if=kali-2.1.2-rpi2.img of=/dev/disk2
```

Eject the SD Card
```
$ sudo diskutil eject /dev/disk2
```


## Step 1.1: Installing full version of Kali Linux

Login to Kali Linux `root@toor`

Change your UNIX passwor
```
$ passwd
```

Update the OS
```
$ apt-get update
```

Set up a partition to use the `unallocated` data
```
$ apt-get install gparted
```

Run gparted
```
$ gparted
```

Resize/Move the partition
```
Mount Point: /
```

Upgrade now (2hr install)
```
$ apt-get upgrade
```

If you want the full version of Kali (4hr install)
```
$ apt-get install kali-linux-full
```


## Step 1.2: AutoLoging to kali
https://www.youtube.com/watch?v=U5UkLPb7f8w


## Step 1.3: VNC Connection

Install X11VNC
```
apt-get install x11vnc
```

Add the Login Password
```
x11vnc -storepasswd
```

https://www.youtube.com/watch?v=mMenoMyCUi8
