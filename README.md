# rip-notes
notes for raspberry pi

## setup NAS

### updates
```
sudo apt-get update
sudo apt-get upgrade

```
### File system supports
```
sudo apt-get install ntfs-3g

sudo apt-get install exfat-utils exfat-fuse

sudo apt-get install samba samba-common-bin
> if failed, install this dependency and try again:
> sudo apt-get install libldb1=2:1.5.1+really1.4.6-3

```
### Create the NAS folder
```
sudo mkdir /RPINAS
$ sudo chmod 777 /RPINAS

```
### Mount the external USB drive
```

lsblk

NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0  3.7T  0 disk 
|-sda1        8:1    0  200M  0 part 
`-sda2        8:2    0  3.7T  0 part 
mmcblk0     179:0    0 59.5G  0 disk 
|-mmcblk0p1 179:1    0  256M  0 part /boot
`-mmcblk0p2 179:2    0 59.2G  0 part /

sudo mount /dev/sda2 /RPINAS/

```
### Setup Samba network folder
```

sudo vi /etc/samba/smb.conf

[Raspberry Pi 4_01 NAS Server]
comment = "Pi4-01"
path = /RPINAS
read only = no
writeable = yes
browseable = yes
create mask = 0777
directory mask = 0777
public = no
force user = root

```
### Create NAS user
```
sudo adduser apirut

sudo smbpasswd -a apirut

```
### Restart Samba
```

sudo /etc/init.d/smbd restart
sudo /etc/init.d/nmbd restart

lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0  3.7T  0 disk 
|-sda1        8:1    0  200M  0 part 
`-sda2        8:2    0  3.7T  0 part /RPINAS
mmcblk0     179:0    0 59.5G  0 disk 
|-mmcblk0p1 179:1    0  256M  0 part /boot
`-mmcblk0p2 179:2    0 59.2G  0 part /

```
### Setup FSTAB for auto mount
```

sudo vi /etc/fstab

/dev/sda2 /RPINAS auto defaults, user 0 2

```
### Setup static IP
```

sudo vi /etc/dhcpcd.conf

# setting static ip address
static ip_address=192.168.1.125/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1

umount /RPIRAS

sudo reboot

```
### NOTES
```

THE CURRENT MOUNT POINT OF SSD256:

sudo mount /dev/sda4 /RPINAS/

lsblk

NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0 232.9G  0 disk 
|-sda1        8:1    0   529M  0 part 
|-sda2        8:2    0    99M  0 part 
|-sda3        8:3    0    16M  0 part 
`-sda4        8:4    0 232.3G  0 part /RPINAS
mmcblk0     179:0    0  59.5G  0 disk 
|-mmcblk0p1 179:1    0   256M  0 part /boot
`-mmcblk0p2 179:2    0  59.2G  0 part /

```

### *** WARNING:
When the external drive not connected to the pi, it will boot up /w ‘root account locked’ error mode:
https://samx18.io/blog/2017/11/05/piBootIssue.html



## Get CPU temperature - python
```
pip install gpiozero
```
```
from gpiozero import CPUTemperature
cpu = CPUTemperature()
print(cpu.temperature)
```

## Get IP Address - python
```
import socket

def get_ip_address():
    ip_address = ''
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.connect(("8.8.8.8",80))
    ip_address = s.getsockname()[0]
    s.close()
    return ip_address
```

## Configure Raspberry PI
```
sudo raspi-config 
```

## SSH port forwarding
```
ssh -L 1441:localhost:1441 -L 1442:localhost:1442 [remote host username]@[remote host IP]
```

## Install Java 

Default SDK
```
sudo apt update
sudo apt install default-jdk
java -version
```

Java 8
```
sudo apt update
sudo apt install openjdk-8-jdk
java -version
```

Uninstall Java
```
sudo apt remove default-jdk
```

More info:
https://linuxize.com/post/install-java-on-raspberry-pi/


## Change APT-GET mirror site

Mirror site list:  
https://www.raspbian.org/RaspbianMirrors

Change the mirrow site:  
```
sudo vi /etc/apt/sources.list
```
Edit the file as the below:

```
# AV : change to specfic mirror server
# DEFAULT
#deb http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi
# Khonkhan University
#deb http://mirror.kku.ac.th/raspbian/raspbian/ buster main contrib non-free rpi
# NUS School of Computing, Singapore
deb http://mirror.nus.edu.sg/raspbian/raspbian/ buster main contrib non-free rpi
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi
```
more info here:
https://tinylab.page/raspberry-pi-err-1-http-mirror1-ku-ac-th-fetch/

## check Pi model
```
cat /proc/device-tree/model  
```

## CPU Info
```
cat /proc/cpuinfo
```

## IP address
```
hostname -I
```

## activate google voice hat
```
sudo nano /boot/config.txt  
add these two lines to the end of the file:  
dtoverlay=i2s-mmap  
dtoverlay=googlevoicehat-soundcard  
reboot the pi  
```

## Fliping upside-down rpi 7 inch touch screen

Download sh script from GitHub  
```
sudo wget -P /tmp https://raw.githubusercontent.com/remonlam/rpi-touch-display-fix/master/rpi-touch-display-fix.sh
```

Make file executable  
```
sudo chmod 755 /tmp/rpi-touch-display-fix.sh
```

Run script  
```
/tmp/./rpi-touch-display-fix.sh
```

## set screen resolution
```
https://www.raspberrypi.org/forums/viewtopic.php?f=108&t=184762
```

## start GUI
```
startx
```

## see IP address
```
ip addr show
```

## shutdown/reboot
```
sudo shutdown now  
sudo reboot
```

## updates & upgrades
```
sudo apt-get update  
sudo apt-get upgrade
```

## homebridge on rpi
```
https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi
```

## configure waveshare 5 inch screen
```
https://www.waveshare.com/wiki/5inch_HDMI_LCD_(B)  
append to following settings to /boot/config.txt  
max_usb_current=1  
hdmi_group=2  
hdmi_mode=87  
hdmi_cvt 800 480 60 6 0 0 0  
hdmi_drive=1  
```

## enable sound
```
add the following to /boot/config.txt
dtparam=audio=1  
```

uninstall PulseAudio
```
sudo apt-get remove PulseAudio ??
```

## static IP
```
sudo nano /etc/dhcpcd.conf at the command prompt  
```

Scroll to the bottom of the script, and add the following lines:
```
interface eth0
static ip_address=192.168.0.2/24  
static routers=192.168.0.1  
static domain_name_servers=192.168.0.1  

interface wlan0
static ip_address=192.168.0.2/24  
static routers=192.168.0.1  
static domain_name_servers=192.168.0.1  
```

## auto start script under pi user
```
sudo vi /home/pi/.bashrc  
echo "  >> starting Google assistant in 5 seconds, ^c to cancel ..."  
sleep 5  
/home/pi/python/voicekit-pi-gpio/voice_gpio.py

sudo raspi-config
> enable auto boot login for the text console
```

## which Wifi SSID being connected to
```
iwconfig
```

## connect to specific Wifi
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

## installing I2C LCD
enable I2C interface on raspberry pi  
```
sudo raspi-config  
```

INSTALL I2C-TOOLS AND SMBUS  
```
sudo apt-get install i2c-tools  
sudo apt-get install python-smbus  
```
http://www.circuitbasics.com/raspberry-pi-i2c-lcd-set-up-and-programming/

## reading temperature sensor
```
git clone https://github.com/adafruit/Adafruit_Python_DHT.git  
cd Adafruit_Python_DHT  
sudo python setup.py install  
```
http://www.circuitbasics.com/how-to-set-up-the-dht11-humidity-sensor-on-the-raspberry-pi/

## LCD Matrix

software:  
```
$ git clone https://github.com/hzeller/rpi-rgb-led-matrix/
$ make  
$ sudo ./demo -t 15 -D 1 runtext.ppm --led-rows=32 --led-cols=64 --led-chain=2  
```
product page:  
https://www.dfrobot.com/wiki/index.php/64x32_RGB_LED_Matrix_-_4mm_pitch_SKU:DFR0460  

How to and software installation:  
https://learn.adafruit.com/connecting-a-16x32-rgb-led-matrix-panel-to-a-raspberry-pi/testing

Hardware indepth information:  
https://github.com/hzeller/rpi-rgb-led-matrix/blob/master/wiring.md


## Installing Docker

1. Install Docker
```
curl -sSL https://get.docker.com | sh
```

2. Add permission to Pi User to run Docker Commands
```
sudo usermod -aG docker pi
```

Reboot here or run the next commands with a sudo

3. Test Docker installation
```
docker run hello-world
```

4. IMPORTANT! Install proper dependencies
```
sudo apt-get install libffi-dev libssl-dev

sudo apt-get install -y python python-pip

sudo apt-get remove python-configparser
```

5. Install Docker Compose
```
sudo pip install docker-compose
```
https://dev.to/rohansawant/installing-docker-and-docker-compose-on-the-raspberry-pi-in-5-simple-steps-3mgl

## Installing Node JS

https://www.instructables.com/id/Install-Nodejs-and-Npm-on-Raspberry-Pi/
```
sudo apt-get install xz-utils

unxz node-v12.14.1-linux-armv7l.tar.xz

tar -xf node-v12.14.1-linux-armv7l.tar
```

## Enable Bluetooth on Ubuntu
```
symptom: no bluetooth device found
sudo hciattach /dev/ttyAMA0 bcm43xx 921600
```

## measure core temperature

https://www.raspberrypi.org/documentation/linux/usage/cron.md
```
vcgencmd measure_temp
```

## cron

https://www.raspberrypi.org/documentation/linux/usage/cron.md

https://crontab.guru/every-1-hour

## USB Ethernet

https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/

## install Pycharm

https://www.element14.com/community/community/raspberry-pi/blog/2019/09/12/installing-pycharm-on-raspberry-pi

## Setup Message Board

https://toms3d.org/2019/09/19/building-a-digital-dashboard-software-setup/

## Disable chromium can't update message

add the following line to: /etc/chromium-browser/customizations/01-disable-update-check
```
CHROMIUM_FLAGS="${CHROMIUM_FLAGS} --check-for-update-interval=31536000"
```

