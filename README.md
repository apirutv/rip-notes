# rip-notes
notes for raspberry pi

## Fliping upside-down rpi 7 inch touch screen

Download sh script from GitHub  
sudo wget -P /tmp https://raw.githubusercontent.com/remonlam/rpi-touch-display-fix/master/rpi-touch-display-fix.sh

Make file executable  
sudo chmod 755 /tmp/rpi-touch-display-fix.sh

Run script  
/tmp/./rpi-touch-display-fix.sh


## set screen resolution
https://www.raspberrypi.org/forums/viewtopic.php?f=108&t=184762

## start GUI
startx

## see IP address
ip addr show

## shutdown/reboot
sudo shutdown now  
sudo reboot

## updates & upgrades
sudo apt-get updates
sudo apt-get upgrades

## homebridge on rpi
https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi

## configure waveshare 5 inch screen
https://www.waveshare.com/wiki/5inch_HDMI_LCD_(B)  
append to following settings to /boot/config.txt  
max_usb_current=1  
hdmi_group=2  
hdmi_mode=87  
hdmi_cvt 800 480 60 6 0 0 0  
hdmi_drive=1  

## enable sound
add the following to /boot/config.txt
dtparam=audio=1  

uninstall PulseAudio
sudo apt-get remove PulseAudio ??

## static IP
Type sudo nano /etc/dhcpcd.conf at the command prompt  

Scroll to the bottom of the script, and add the following lines:

interface eth0
static ip_address=192.168.0.2/24  
static routers=192.168.0.1  
static domain_name_servers=192.168.0.1  

interface wlan0
static ip_address=192.168.0.2/24  
static routers=192.168.0.1  
static domain_name_servers=192.168.0.1  

## auto start script under pi user

sudo vi /home/pi/.bashrc  
echo "  >> starting Google assistant in 5 seconds, ^c to cancel ..."  
sleep 5  
/home/pi/python/voicekit-pi-gpio/voice_gpio.py

## which Wifi SSID being connected to

iwconfig

## connect to specific Wifi

sudo nano /etc/wpa_supplicant/wpa_supplicant.conf









