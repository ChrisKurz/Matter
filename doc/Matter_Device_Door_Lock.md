# Example Matter Device: Door Lock

The sample description can be found here:

https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/matter/lock/README.html#

## Realize Door Lock on an nRF52840DK

This description is using nRF Connect SDK V1.8.0. Note that later versions may differ from the example delivered with this SDK version!

The quickest way to setup the board is to use command line instructions. Here is the description:

1. Open nRF Connect for Desktop and start Toolchain Manager

2. As soon as a nRF Connect SDK version 1.8.0 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

NOTE: Please use nRF Connect SDK Version 1.8.0. Some needed files are not available in nRF Connect SDK V1.9.0 or later!!!

3. go to the folder __nrf/samples/matter/lock__

       cd nrf/samples/matter/lock

4. in case there is a build folder, remove it.

       rm -rf build
       
5. build the project:

       west build -b nrf52840dk_nrf52840

6. connect the nRF52840DK development board to the computer andfalsh the firmware to it:

       west flash --erase

7. Open a terminal program (e.g. Putty) and see logs from the development board. Default settings for terminal program: 115200 Bd, 8 databits, 1 stop bit, no parity


## Commissioning of the Matter Accessory using Matter Controller
You must provide the Matter Controller with network credentials, which will be further used during device commissioning procedure to configure the device with a Thread network.

1. Write down the discriminator and setup PIN code:

The Matter Controller uses a 12-bit value called __discriminator__ to discern between multiple commissionable device advertisements, as well as a 27-bit __setup PIN code__ to authenticate the device. You can find these values in the UART logging terminal of the device (e.g. Putty). Here is an example how the output in the terminal might look like:

       I: 254 [DL]Device Configuration:
       I: 257 [DL] Serial Number: TEST_SN
       I: 260 [DL] Vendor Id: 9050 (0x235A)
       I: 263 [DL] Product Id: 20043 (0x4E4B)
       I: 267 [DL] Hardware Version: 1
       I: 270 [DL] Setup Pin Code: 20202021
       I: 273 [DL] Setup Discriminator: 3840 (0xF00)
       I: 278 [DL] Manufacturing Date: (not set)
       I: 281 [DL] Device Type: 65535 (0xFFFF)
  
In this example you find the following parameters:
- Discriminator of this device is:  3840
- Setup code of this device is:  20202021


2. First, fetch and store the current Active Operational Dataset from the OpenThread Border Router (OTBR). In this example the OTBR is running on Docker, so we have to enter the following:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset active -x"

Note: we will need the hex number later. For example, copy it into notpad. 

3. get the PAN-ID of your thread network:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset extpanid"

Note: we will need this info later. Note it somewhere. 

4. start Matter Controller again by entering:

        chip-device-ctrl

5. Press button 4 on nRF52840DK. (this starts Bluetooth LE advertising on Matter Accessory device. The Advertising will be done for approximately 15 minutes!)

6. Connecting via BLE:

        connect -ble 3840 20202021 1

Note: You can use the command "ble-scan" to check if the Matter Accessory is still advertising. If advertising has stop, repeat step 4. 

7. The statement "Secure Session to Device Established" has to be shown in UART log.

8. Adding Thread network:

        zcl NetworkCommissioning AddThreadNetwork 1 0 0 operationalDataset=hex:"USE YOUR HEX-DATASET HERE" breadcrumb=0 timeoutMs=3000

Note: replace the string **"USE YOUR HEX-DATASET HERE"** in above command by the active operational dataset you read in step 1.

9. Enable Thread network:

        zcl NetworkCommissioning EnableNetwork 1 0 0 networkID=hex:"USE YOUR EXTPAN-ID HERE" breadcrumb=0 timeoutMs=3000
       
Note: replace the string **"USE YOUR EXTPAN-ID HERE"** in above command by the extended PAN-Id you read in step 2.

10. The BLE connection is no longer needed. You can close it with following command:
       
       close-ble

11. on the UART log terminal enter following command to check if the Matter Accessory is part of the Thread network. It should be in the state "child". 


## Control Application ZCL Clusters
Execute the following command to toggle the LED state:

       chip-device-ctrl > zcl OnOff Toggle 1234 1 0

You get some details about the OnOff cluster by entering following command:

       chip-device-ctrl > zcl ? OnOff
       
## Read Basic Information out of Matter Accessory
Every Matter accessory device supports a Basic Cluster, which maintains collection of attributes that a controller can obtain from a device, such as the vendor name, the product name, or software version. Use zclread command to read those values from the device:

       chip-device-ctrl > zclread Basic VendorName 1234 1 0
       chip-device-ctrl > zclread Basic ProductName 1234 1 0
       chip-device-ctrl > zclread Basic SoftwareVersion 1234 1 0
       
Use the following command to list all available commands for Basic Cluster:

       zcl ? Basic


[go to previous page](../README.md)
