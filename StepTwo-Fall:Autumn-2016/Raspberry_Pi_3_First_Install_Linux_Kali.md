## Raspberry Pi 3 First Install of Kali Linux

## Step 1.0: Getting Started

0. Format The SD Card with [SDFormatter](https://www.sdcard.org/downloads/formatter_4/index.html)

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
$ sudo dd bs=1m if=kali-2.1.2-rpi2.img of=/dev/disk2
```

4. Eject the SD Card
```
$ sudo diskutil eject /dev/disk2
```


## Step 1.1: Installing full version of Kali Linux

1. Login to Kali Linux `root@toor`


2. Change your UNIX passwor
```
$ passwd
```

3. Update the OS
```
$ apt-get update
```

4. Set up a partiaion to use the `unallocated` data
```
$ apt-get install gparted
```

5. Run gparted
```
$ gparted
```


6. Resize/Move
```
Mount Point: /
```

7. Upgrade now (2hr install)
```
$ apt-get upgrade
```

8. If you want the full version of Kali (4hr install)
```
$ apt-get install kali-linux-full
```


## Step 1.2: AutoLoging to kali
https://www.youtube.com/watch?v=U5UkLPb7f8w


## Step 1.2: VNC Connection

1. Install X11VNC
```
apt-get install x11vnc
```

2. Add the Login Password
```
x11vnc -storepasswd
```

https://www.youtube.com/watch?v=mMenoMyCUi8
