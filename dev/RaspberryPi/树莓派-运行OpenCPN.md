### 树莓派-运行OpenCPN

http://opencpn.org/ocpn/compiling_source_linux
https://mvcesc.wordpress.com/2013/09/20/detailed-raspberry-pi-setup-documentation/
http://www.agurney.com/raspberry-pi/pi-chart

最新：OpenCPN (Version 5.0 - updated 26 March 2019)



```

Pi Chart

... openCPN, Grib and Wefax on Raspberry Pi


OpenCPN (updated 22 Feb 2017)

You can build your own version of OpenCPN using the instructions on opencpn.org (or see below), or download and install a pre-built version that I've created.

--------

The following describes an installation of OpenCPN on a Raspberry Pi using a pre-built .deb package if you are not comfortable with building your own:

- download one of my packages:
- version 4.5.221 (built with GSHHS=HIGH, Feb 23rd 2017 on Raspberry Pi 2)
- version 3.1.1309: (original Raspberry Pi)
- version 3.2.0
- Version 4.0.0-1 (compiled on Raspberry Pi 2) 4MB (vanilla)
- Version 4.0.0-1 (tides and lo-res world map - compiled on Raspberry Pi 2) 6MB
- 4.2.0-1 with CRUDE maps (9MB)
- 4.2.0-1 with HIGH resolution maps, tides and docs (45MB)

- Copy the file to your Raspberry Pi (anywhere, for example /home/pi)
.. alternatively, if your Pi is connected to the Internet you can pull it directly from the command line (see wget, below)
- Install dependencies
- Install the package
- Update the config file
- Update the Pi's config file
- Copy your charts (anywhere you want, I use /usr/local/share/charts )
- Run OpenCPN (under the Education menu in the UI, or enter 'opencpn' in a terminal window)
- Identify your chart locations
- Setup your personal settings under Options

Here's a transcript of an install session:

cd /home/pi
mkdir opencpn 
cd opencpn/ 
wget http://agurney.com/raspi/opencpn_4.5.221-1_armhf.deb


If this is your first installation then you'll need to install some dependencies, as follows:

sudo apt-get -y install cmake gettext \
  gpsd gpsd-clients libgps-dev wx-common \
  libwxgtk3.0-dev libgtk2.0-dev wx3.0-i18n \
  wx3.0-headers libbz2-dev libtinyxml-dev \
  portaudio19-dev libcurl4-openssl-dev libcairo2-dev

sudo dpkg -i opencpn_4.5.221-1_armhf.deb




Add a couple of lines to the /boot/config.txt file; use any text editor such as vi, nano or leafpad.
These changes will be effective following the next reboot and are required to resolve problems that the Pi/armhf has with vector charts.

vi /boot/config.txt


and insert the following lines (under existing framebuffer entries for convenience)

  framebuffer_depth=32
  framebuffer_ignore_alpha=1
/pre>






you may find it useful to edit your ~/.opencpn.opencpn.conf file and add values for MEMCacheLimit and/or NCacheLimit .. see opencpn.org for details.




DIY

Refer to the opencpn.org if you want to build the latest OpenCPN for yourself , 
however, the following are the instructions that I use. Copy and paste the information, don't run it as a script!




######################################################
# Build and Install opencpn on Raspberry PI
# 23 Feb 2017 by opencpn@agurney.com 
# refer to http://opencpn.org/  
######################################################
# 2017-01-11-raspbian-jessie 
######################################################"


# download latest full Raspbian image (not Lite) 
# https://www.raspberrypi.org/downloads/raspbian/
# for example 2017-01-11-raspbian-jessie.zip
#
# extract the zip to give the .img file, for example 
# 2017-01-11-raspbian-jessie.img
# write an SD card with this image, for example 
# using Win32DiskImager
#
# If you intend accessing or configuring the Pi remotely
# then create a file called 'ssh' in the root of the SD card
# connect your pi to local network
#
# insert the SD card and boot the Pi 
# use a keyboard, or if it's on a wired network SSh into 
# the Pi using / Kitty (Putty)
#
# - If you are working with a screen attached to the Pi you'll 
# be logged into the window manager - open a terminal window
# - If you are working remotely, SSH into the Pi with a client 
# such as Kitty or Putty
#

sudo raspi-config
# the way this works keeps changing, but you want to:
# - Change the Pi user's password
# - Expand filesystem 
# - disable the serial console
# - enable the serial interface
# - probably enable the I2C interface (if you have sensors, 
# in my case a GPS chip, temperature, pressure and more)
# - probably enable GL graphics 
# + any other changes that you want

# Reboot the Pi
# login (changed password)



# Add a couple of lines to the /boot/config.txt file; 
# use any text editor such as vi or leafpad.
# These changes will be effective following the next 
# reboot and are required to resolve problems that the 
# Pi/armhf has with vector charts. 
#
# Edit /boot/config.txt
sudo nano /boot/config.txt

# Add the following lines to /boot/config.txt
  framebuffer_depth=32
  framebuffer_ignore_alpha=1

# update installed packages
sudo apt-get update
sudo apt-get upgrade

# update your firmware
sudo BRANCH=next rpi-update

# install dependencies ... copy and paste the next five 
# lines in one operation (\ concatenates the commands)
sudo apt-get -y install build-essential cmake \
gettext git-core libgps-dev wx-common libwxgtk3.0-dev \
libglu1-mesa-dev libgtk2.0-dev wx3.0-headers libbz2-dev \
libtinyxml-dev libportaudio2 portaudio19-dev \
libcurl4-openssl-dev libexpat1-dev libcairo2-dev \ 
gpsd gpsd-clients liblzma-dev libelf-dev \

# retrieve OpenCPN source and build it

cd ~
git clone git://github.com/OpenCPN/OpenCPN.git

#
# copy and unpack gshhs and tcdata from 
# https://sourceforge.net/projects/opencpnplugins/files/opencpn_packaging_data/
# copy or move the directories to the opencpn data directory 
# so you end up with, for example, 
# /home/pi/OpenCPN/data/tcdata
# /home/pi/OpenCPN/data/gshhs  
#

cd ~/OpenCPN/
mkdir build
cd build

# OpenCPN With basic map only:
cmake -DBUNDLE_GSHHS=CRUDE ../

# OR with High resolution world map
cmake -DBUNDLE_DOCS=ON -DBUNDLE_TCDATA=ON -DBUNDLE_GSHHS=HIGH ../

make 
## You can try using more cores to speed it up, 
## but my Pi2 kept running out of memory using make -j2 
## a Pi3 should have better luck.

sudo make install

# might as well create a .deb packagae so you can restore 
# without having to rebuild
sudo make package

# now save the package so it can be used as a backup
# ... export the file (cp, FTP) 
# ~/OpenCPN/build/_CPack_Packages/Linux/DEB/opencpn_4.5.221-1_armhf.deb 
# (or whatever it's called) to external storage
#
# You should be good to go after a reboot

opencpn

# Open connections in OpenCPN setup to configure GPS
# if you connect a USB GPS, and there are no other USB devices,
# then it will probably be at /dev/ttyUSB0 and/or /dev/serial0.
# If you use a SIRF device connected to the I2C pins, or similar, 
# OpenCPN will complain that it neds a serial device, even though 
# you can probably see NMEA data coming in on /dev/serial0 
# and/or /dev/ttyAMA0 .. in this case make us of gpsd by 
# setting Network, and the host IP to 'localhost'.
#
# Next, create a directory and copy your charts, for example

sudo mkdir /usr/local/share/charts
sudo chown pi:pi /usr/local/share/charts








Free charts for the US are available from NOAA, You'll have to purchase charts for the rest of the world or scan your own; refer to opencpn.org for supported formats.





If, like me, you sail the West coast of Scotland I can recommend Antares charts .










Grib - zyGrib






There is a version of zyGrib available in the Raspbian repository (installed using the command apt-get install zygrib)


.. unfortunately, at time of writing this is obsolete and you will be prompted to upgrade (which isn't an option!)


So.... I've adapted the script that first appeared here.


login as the pi user and go the home directory (/home/pi)


cd ~
download the script


wget http://agurney.com/raspi/zygrib_install.sh
change the permissions to make the script executable


chmod +x zygrib_install.sh






sudo apt-get update












run the script






./zygrib_install.sh












... it will take around 90 minutes to complete if compiling on a Pi.






Wefax, Navtex and RTTY

















WEFAX Only






If you only want wefax, then hamfax is simple to install:






sudo apt-get install hamfax






WEFAX, NAVTEX and other data modes






If you want more flexibility, and modes other than Wefax, try fldigi






sudo apt-get install fldigi






SOUND:






You'll need an audio input device, presumably a USB dongle.
If you've connected an audio dongle the chances are that hamfax won't work because it sees the wrong device, 
you can check by running alsamixer from the command line.
Press f6 to view the sound cards, you'll probably see something like this, where the onboard device is default:






  ┌─────── Sound Card ──────
  │- (default)
  │0 bcm2835 ALSA
  │1 Generic USB Audio Device
  │ enter device name...
  └───────────────────────






We can force the USB device to be default as follows:






vi /etc/modules






append the line:






snd-usb-audio






vi /etc/modprobe.d/alsa-base.conf






find the line






options snd-usb-audio index=-2






and change the -2 (or whatever) to 0






options snd-usb-audio index=0






After a reboot, run alsamixer again and change the MIC level so that audio's available to the apps.

































Sorry about the adverts around the page, but the few coppers the clicks bring in help towards the upkeep of the site.
```

