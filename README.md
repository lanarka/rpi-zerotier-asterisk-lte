# rpi-zerotier-asterisk-lte

A few simple steps to bake a simple portable PBX 

## Features
- 4G LTE connection
- secured private network
- internal extensions
- outgoing and incoming calls through VOIP operator


![Rpi4](images/photo.jpg)


### What you will need

- RaspberryPi 3 or 4 (I have tested RPi4 with 4GB RAM)
- a power supply for the RaspberryPi 
- USB dongle (I am using the Huawei E3372)
- SD Card
- sim card with internet connection


### Prepare SD card
#### Download image and burn SD card
```sh
	wget https://downloads.raspberrypi.com/raspios_arm64/images/raspios_arm64-2023-05-03/2023-05-03-raspios-bullseye-arm64.img.xz
	xz -d -T0 2023-05-03-raspios-bullseye-arm64.img.xz
	sudo dd bs=4M if=2023-05-03-raspios-bullseye-arm64.img of=/dev/mmcblk0
```
#### Setup password and enable SSH
- Create password hash
```sh
		echo 'mypassword' | openssl passwd -6 -stdin
```
- Create empty file `ssh` 
- Create file `userconf` with content
		`pi:PASSWORDHASH`
- Mount SD card and copy both files to /boot folder

### Instalation

#### Prepare operating system

```sh
	sudo apt update && sudo apt upgrade
	sudo reboot
```

#### Install prerequisities
 
```sh
	sudo apt install joe
```
#### Install asterisk
```sh
	sudo apt install asterisk asterisk-dahdi
```

#### Configure asterisk
- Copy `extensions.conf` and `sip.conf` to `/etc/asterisk`

```sh
	asterisk -r
```
```	
	> sip show peers
	> core restart now
	> sip show registry
```

#### Restart asterisk
```sh
	sudo systemctl restart asterisk

```

#### Install zerotier
```sh
   curl -s https://install.zerotier.com | sudo bash
```

#### Setup zerotier
```sh   
   sudo service zerotier-one status
   sudo zerotier-cli join NETWORK-ID
```

- invite user ID in zerotier control panel
- check zerotier ethernet adapter
- try ping to other device

#### Connect 4G LTE dongle

- no additional drivers needed for E3372
- check adapter eth1 configuration (dhcp)
- check internet connection
- reboot

