# Create an OpenThread Border Router (OTBR)

needed Hardware:   Raspberry Pi 3B (or newer) with SD card of at least 8 GB of memory

The OpenThread Border Router is installed on a RaspberryPi by following these steps:

## Prepare the Raspberry Pi

1. Install Ubuntu 64-bit server OS (20.04.3 LTS) on a Raspberry's SD card using rpi-imager tool:  https://www.raspberrypi.com/software

(__NOTE:__ Ubuntu default login: __ubuntu__  /  password: __ubuntu__)

2. Ensure the RaspberryPi has internet access (e.g. Ethernet connection) and update/upgrade the system:

       sudo apt-get update && sudo apt-get upgrade

------

__NOTE__

Remote control of Raspberry Pi for Windows users:

a. Open Toolchan Manager

b. Expand the dropdown list at the latest NCS version (button with arrow) and click on "Open bash"

c. Run the following command in the bash:  

       ssh ubuntu@ <IP-address of RaspberryPi>

------

3. Install all required packages for the Raspberry Pi (20.04.3 LTS):

       sudo apt install -fy python3-pip pkg-config libavahi-client-dev libcairo2-dev libgirepository1.0-dev avahi-daemon pi-bluetooth avahi-utils

__NOTE:__ You can check the Ubuntu version of Raspberry Pi with following instruction:  

       lsb_release -a

Aditional module need to be installed for Ubuntu 20.10: sudo apt install linux-modules-extra-raspi


4. Some of this software packages require a rebot of the Raspberry Pi. Enter following instruction for reboot: 
 
       sudo reboot


## Prepare Radio Co-Processor (RCP)

The Raspberry Pi does not directly support Thread communication. However, Thread communication is possible when a Radio Co-Processor (RCP) is used. The following steps describe how to setup the RCP.

5. Different development tools are supported here. Moreover, there are different ways to build the firmware for the RCP device. Please select one of the following possibilites:
- ![Tool: nRF52840dongle building: using Command Line tool](Create Create an OpenThread Border Router_RCP-dongle-west.md)
- Tool: nRF52840dongle / building: using Visual Studio Code
- Tool: nRF52840DK / building: using Command Line tool ("west" tool)
- Tool: nRF52840DK / building: using Visual Studio Code




 
