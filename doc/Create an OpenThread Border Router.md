# Create an OpenThread Border Router (OTBR)

needed Hardware:
- Raspberry Pi 3B (or newer) with SD card of at least 8 GB of memory

The OpenThread Border Router is installed on a RaspberryPi by following these steps:

1. Install Ubuntu 64-bit server OS (20.04.3 LTS) on a Raspberry's SD card using rpi-imager tool:  https://www.raspberrypi.com/software

------

__NOTE__

Ubuntu default login credentials:   
- login: __ubuntu__
- password: __ubuntu__

------


2. Ensure the RaspberryPi has internet access (e.g. Ethernet connection) and update/upgrade the system:

       sudo apt-get update && sudo apt-get upgrade

------

__NOTE__

Remote control of Raspberry Pi for Windows users:

1. Open Toolchan Manager
2. Expand the dropdown list at the latest NCS version (button with arrow) and click on "Open bash"
3. Run  

       ssh ubuntu@ <IP-address of RaspberryPi>

------

3. Install all required packages for the Raspberry Pi (20.04.3 LTS):

       sudo apt install -fy python3-pip pkg-config libavahi-client-dev libcairo2-dev libgirepository1.0-dev avahi-daemon pi-bluetooth avahi-utils

------

__NOTE__

Use following instruction to check the Ubuntu version of Raspberry Pi:

       lsb_release -a

Aditional module need to be installed for Ubuntu 20.10:

       sudo apt install linux-modules-extra-raspi

------

4. Some of this software packages require a rebot of the Raspberry Pi. Enter following instruction for reboot: 
 
       sudo reboot


 
