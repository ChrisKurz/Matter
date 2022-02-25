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


## Matter Device Commissioning
The Matter Controller uses a 12-bit value called __discriminator__ to discern between multiple commissionable device advertisements, as well as a 27-bit __setup PIN code__ to authenticate the device. You can find these values in the logging terminal of the device (e.g. Putty). Here is an example how the output in the terminal might look like:

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
- Temporary Node ID: 1234

Check these parameters on your setup and use your values if they differ. 

Run the following command to establish the secure connection over Bluetooth LE, using the parameters you found out before:

       chip-device-ctrl > connect -ble 3840 20202021 1234

Note: you can skip the last parameter, the Node ID, in the command. If you skip it, the controller will assign it randomly. In that case, note down the Node ID, because it is required later in the configuration process. 

After connecting the device over Bluetooth LE, the controller will go through the following stages:
1) Establishing a secure connection that completes the PASE (Password-Authenticated Session Establishment) session using SPAKE2+ protocol and results in printing the following log:

       ...
       Secure Session to Device Established
       ...

2) Providing the device with a network interface using ZCL Network Commissioning cluster commands, and the network pairing credentials set in the previous step.
3) Discovering the IPv6 address of the Matter accessory using the SRP (Serive Registration Protocol) for Thread devices. It results in printing log that indicates that the node address has been updated. The IPv6 address of the device is cached in the controller for later usage.
4) Closing the Bluetooth LE connection, as the commissioning process is finished and the Pytion CHIP controller is now using only the IPv6 traffic to reach the device.

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
