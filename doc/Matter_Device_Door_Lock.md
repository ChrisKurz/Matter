# Example Matter Device: Door Lock

The sample description can be found here:

https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/matter/lock/README.html#

## Realize Door Lock on an nRF52840DK

[Video Link](https://youtu.be/unZ_Lrzg6fg)

This description is using nRF Connect SDK V1.8.0. Note that later versions may differ from the example delivered with this SDK version!

The quickest way to setup the board is to use command line instructions. Here is the description:

1. Open nRF Connect for Desktop and start Toolchain Manager

2. As soon as a nRF Connect SDK version 1.8.0 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

NOTE: Please use nRF Connect SDK Version 1.8.0. 

3. go to the folder __nrf/samples/matter/lock__

       cd nrf/samples/matter/lock

4. in case there is a build folder, remove it.

       rm -rf build
       
5. build the project:

       west build -b nrf52840dk_nrf52840

6. connect the nRF52840DK development board to the computer andfalsh the firmware to it:

       west flash --erase

7. Open a terminal program (e.g. Putty) and see logs from the development board. Default settings for terminal program: 115200 Bd, 8 databits, 1 stop bit, no parity

## Restart OpenThread Border Router (OTBR) and Matter Controller
[Video Link](https://youtu.be/XH7BlWnV-iY)

You can skip the steps in this subchapter if you just installed the Raspberry Pi and if no Reset or power cycling of the Raspberry Pi was done. In this case continue with step 12.

Otherwise follow these steps:

8. Ensure the RaspberryPi has internet access (Ethernet connection). Remote control of Raspberry Pi for Windows users:

a. Open Toolchan Manager

b. Expand the dropdown list at the latest NCS version (button with arrow) and click on "Open bash"

c. Run the following command in the bash:  

    ssh ubuntu@ <IP-address of RaspberryPi>

9. Start OTBR docker container:

        sudo docker run -it --rm --privileged --name otbr --network host --volume /dev/ttyACM0:/dev/radio nrfconnect/otbr:8ae81c5 --radio-url spinel+hdlc+uart:///dev/radio?uart-baudrate=1000000

__NOTE:__ For a new Raspberry setup the serial interface is usually ttyACM0. You might have to check if ttyACOM0 is the right serial interface. This is done by entering following command: 
   
    ls /dev// | grep ttyACM*

10. open a second bash window (in the same way as described in step 8).
11. Run the Python CHIP controller application on the Raspberry Pi:

        chip-device-ctrl

## Ensure the OTBR is running as Leader in Thread network
[Video Link](https://youtu.be/SfkcsiwNxPs)

12. Open a browser on a computer that is in the same Network as the Raspberry PI (e.g. laptop is connected via Wifi to a router where the Raspberry Pi is connected via Ethernet cable). 
13. Open the page **http:// < IP address of your Raspberry Pi >**
14. Thread Border Router webpage should now be shown.
15. Click in the menu bar on the left on "Status". In case an "RCP:State disabled" is shown in the first line, you have to form the Thread Network:

a. Go to the "Form" tab and press button "Form"

b. You succeeded with these steps when you see the message "__FORM operation is successful__"

## Commissioning of the Matter Accessory using Matter Controller
[Video Link](https://youtu.be/nIRvKnQqS1A)

You must provide the Matter Controller with network credentials, which will be further used during device commissioning procedure to configure the device with a Thread network.

16. In the terminal program (e.g. Putty) you should see the UART log from the nRF52840DK board. Write down the discriminator and setup PIN code:

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

17. Open a third bash window and run ssh (in the same way as described in step 8).
18. First, fetch and store the current Active Operational Dataset from the OpenThread Border Router (OTBR). In this example the OTBR is running on Docker, so we have to enter the following:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset active -x"

Note: we will need the hex number later. For example, copy it into Notepad. 

19. get the PAN-ID of your thread network:

        sudo docker exec -it otbr sh -c "sudo ot-ctl dataset extpanid"

Note: we will need this info later. Note it somewhere. 

20. Press button 4 on nRF52840DK. (this starts Bluetooth LE advertising on Matter Accessory device. The Advertising will be done for approximately 15 minutes!). Check the UART log for advertising started message.

21. In the bash window that runs the Matter controller (prompt "chip-device-ctrl >" is shown) enter the following to connect via BLE:

        connect -ble 3840 20202021 1

Note: You can use the command "ble-scan" to check if the Matter Accessory is still advertising. If advertising has stop, repeat step 4. 

22. The statement "Secure Session to Device Established" has to be shown in UART log.

23. Adding Thread network. In the Matter Controller execute the following command with your own HEX-DATASET:

        zcl NetworkCommissioning AddThreadNetwork 1 0 0 operationalDataset=hex:"USE YOUR HEX-DATASET HERE" breadcrumb=0 timeoutMs=3000

Note: replace the string **"USE YOUR HEX-DATASET HERE"** in above command by the active operational dataset you read in step 19.

24. Now, enable Thread network in Matter Controller:

        zcl NetworkCommissioning EnableNetwork 1 0 0 networkID=hex:"USE YOUR EXTPAN-ID HERE" breadcrumb=0 timeoutMs=3000
       
Note: replace the string **"USE YOUR EXTPAN-ID HERE"** in above command by the extended PAN-Id you read in step 20.

25. The BLE connection is no longer needed. You can close it with following command in Matter Controller:
       
        close-ble

26. on the UART log terminal enter following command to check if the Matter Accessory is part of the Thread network. It should be in the state "child". 

        ot state

## Control Application ZCL Clusters
27. Execute the following command to toggle the LED state:

        chip-device-ctrl > zcl OnOff Toggle 1 1 0

28. You get some details about the OnOff cluster by entering following command:

        chip-device-ctrl > zcl ? OnOff
       
## Read Basic Information out of Matter Accessory
29. Every Matter accessory device supports a Basic Cluster, which maintains collection of attributes that a controller can obtain from a device, such as the vendor name, the product name, or software version. Use zclread command to read those values from the device:

        chip-device-ctrl > zclread Basic VendorName 1 1 0
        chip-device-ctrl > zclread Basic ProductName 1 1 0
        chip-device-ctrl > zclread Basic SoftwareVersion 1 1 0
       
30. Use the following command to list all available commands for Basic Cluster:

       zcl ? Basic


[go to previous page](../README.md)
