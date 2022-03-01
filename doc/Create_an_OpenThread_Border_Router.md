# Create an OpenThread Border Router (OTBR)

needed Hardware:   Raspberry Pi 3B (or newer) with SD card of at least 8 GB of memory

needed Software:   Note that this description is based on *nRF Connect SDK V1.8.0*! Some modifications in the description here would be needed for newer SDK versions!!!


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

5. Different development tools are supported here. Moreover, there are different ways to build the firmware for the RCP device. Please select one of the following possibilites and follow the step-by-step description:
- [Tool: nRF52840dongle / building: using Command Line tool](Create_an_OpenThread_Border_Router_RCP-dongle-west.md)
- [Tool: nRF52840dongle / building: using Visual Studio Code](Create_an_OpenThread_Border_Router_RCP-dongle-VSC.md)
- [Tool: nRF52840DK / building: using Command Line tool ("west" tool)](Create_an_OpenThread_Border_Router_RCP-DK-west.md)
- [Tool: nRF52840DK / building: using Visual Studio Code](Create_an_OpenThread_Border_Router_RCP-DK-VSC.md)

6. Connect the nRF52840dongle or the nRF52840DK (use Debug USB Connector) to the Raspberry Pi

## Run OTBR on Raspberry Pi

Next, we will install the OTBR software on the Raspberry Pi. Again, here are different ways to do the installation. Here I will use a docker image and run this one on the Raspberry Pi. 

7. First, we have to install docker on the Raspberry Pi. Run following instruction on the Raspberry PI:

       sudo apt update && sudo apt install docker.io
       
8. Now we have to start the docker daemon:

       sudo systemctl start docker
       
9. Download the nrfconnect OpenThread Border Router docker image:

       sudo docker pull nrfconnect/otbr:8ae81c5
       
10. Start OTBR docker container:

        sudo docker run -it --rm --privileged --name otbr --network host --volume /dev/ttyACM0:/dev/radio nrfconnect/otbr:8ae81c5 --radio-url spinel+hdlc+uart:///dev/radio?uart-baudrate=1000000

__NOTE:__ For a new Raspberry setup the serial interface is usually ttyACM0. You might have to check if ttyACOM0 is the right serial interface. This is done by entering following command: 
   
    ls /dev// | grep ttyACM*

## Create Thread Network

Now it should be possible to create a Thread network:

11. Open a browser on a computer that is in the same Network as the Raspberry PI (e.g. laptop is connected via Wifi to a router where the Raspberry Pi is connected via Ethernet cable). 
12. Open the page **http:// < IP address of your Raspberry Pi >**
13. Thread Border Router webpage should now be shown.
14. Go to the "Form" tab and press button "Form"
15. You succeeded with these steps when you see the message "__FORM operation is successful__"

## Adding Python CHIP Controller (Matter Controller)

Now let's add the Matter controller to the Raspberry Pi:

16. Stop Docker by pressing Ctrl+C
17. On Raspberry Pi download controller built packages from sdk-connectedhomeip tag:

        wget https://github.com/nrfconnect/sdk-connectedhomeip/releases/download/v1.8.0/chip-tool-python_linux_debug.zip 

18. Insall unzip, in case unzip was not yet installed on the Raspberry Pi. 

        sudo apt install unzip

19. Extract the .zip package:

        unzip chip-tool-python_linux_debug.zip

20. and install .whl file on Raspberry Pi:

        python3 -m pip install chip-0.0-cp37-abi3-linux_aarch64.whl --force-reinstall

21. Now a reboot is needed:

        sudo reboot
       
22. Try running the Python CHIP controller application on the Raspberry Pi:

        chip-device-ctrl
        
23. You should see now the prompt "chip-device-ctrl > ". Enter help to get a list of available commands. 
24. stop Controller by pressing Ctrl+C

## Set Network Pairing Credentials / Commission the Matter Accessory using Matter Controller
You must provide the Matter Controller with network credentials, which will be further used during device commissioning procedure to configure the device with a Thread network.

1. First, fetch and store the current Active Operational Dataset from the OpenThread Border Router (OTBR). In this example the OTBR is running on Docker, so we have to enter the following:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset active -x"

Note: we will need the hex number later. For example, copy it into notpad. 

2. get the PAN-ID of your thread network:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset extpanid"

Note: we will need this info later. Note it somewhere. 

3. start Matter Controller again by entering:

        chip-device-ctrl

4. Press button 4 on nRF52840DK. (this starts Bluetooth LE advertising on Matter Accessory device. The Advertising will be done for approximately 15 minutes!)

5. Connecting via BLE:

        connect -ble 3840 20202021 1

Note: You can use the command "ble-scan" to check if the Matter Accessory is still advertising. If advertising has stop, repeat step 4. 

6. The statement "Secure Session to Device Established" has to be shown in UART log.

7. Adding Thread network:

        zcl NetworkCommissioning AddThreadNetwork 1 0 0 operationalDataset=hex:"USE YOUR HEX-DATASET HERE" breadcrumb=0 timeoutMs=3000

Note: replace the string **"USE YOUR HEX-DATASET HERE"** in above command by the active operational dataset you read in step 1.

8. Enable Thread network:

        zcl NetworkCommissioning EnableNetwork 1 0 0 networkID=hex:"USE YOUR EXTPAN-ID HERE" breadcrumb=0 timeoutMs=3000
       
Note: replace the string **"USE YOUR EXTPAN-ID HERE"** in above command by the extended PAN-Id you read in step 2.

9. The BLE connection is no longer needed. You can close it with following command:
       
       close-ble

10. on the UART log terminal enter following command to check if the Matter Accessory is part of the Thread network. It should be in the state "child". 
       
       
## 
       
[go to previous page](../README.md)
