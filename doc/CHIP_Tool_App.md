# Using the CHIP Tool Smartphone App

The CHIP Tool Smartphone App is basically realising a Matter Controller on a Smartphone. Using this app together with the provided Matter examples is really easy, as long as the app version fits to the used Matter examples. In the nRF Connect SDK all needed parts are offered. This description is based on nRF Connect SDK Version 1.8.0. So we also have to use the appropriate smartphone app version to ensure it is working with the Matter examples from nRF Connect SDK V1.8.0. 

## Installing CHIP Tool on an Android Smartphone

1) Open the web browser on your Android smartphone (Note: there is only a predefined file for Android smartphones!). 
2) Open the Nordic nRFConnect SDK _Connect Home IP_ repository:  https://github.com/nrfconnect/sdk-connectedhomeip
3) Scroll down, and nearly at the end of the page you find the section "Releases". Click on "Releases".
4) Now you should get a list with different versions. Because we used before the Matter examples from nRF Connect SDK v1.8.0, we also have to use the appropriate CHIP Tool version. So look for V1.8.0 and click on it.
5) You should now see a list of CHIP Tool files. We click on the "chip-tool_android_armv7l.apk" and download the file. After the download is finished the smartphone is asking if it should be installed. Install it!
6) After that you should see the app "CHIPTool" on your smartphone. 

## Adding a Matter Device to a network by using CHIP Tool App
You can also use the CHIP Tool App to add Matter Devices to a network. In case you have setup your Matter Light Bulb and you have added it already to the network, you have to do a factory reset (this erases the stored thread network credentials). Following sub-chapter explains how to do the factory reset:

### Factory Reset of Matter sample Light Buld
7) The Light Bulb example code has this feature included and allows to trigger it by pressing button 1. Note that you have to keep the button 1 pressed for more than 6 seconds. My recommendation is to watch the UART terminal when pressing button 1. After 3 seconds you will get following message, but keep the button pressed when you see this message:

       I: Factory Reset Triggered. Release button within 3000ms to cancel.

If you press long enoug you will see the message "I: Factory Reset triggered" and a restart of the light bulb example will be done.

### Adding Light Buld to network by using Smartphone app (using QR Code)
8) You should see in the UART terminal the boot log. We already used some parameters (Setup Pin Code, and Setup Discriminator). Now we will use a QR Code for the provisioning. Nearly at the end of the log you should find two lines that looks similar to the following:

       I: 485 [SVR]Copy/paste the below URL in a browser to see the QR Code:<CR>
       https://dhrishi.github.io/connectedhomeip/qrcode.html?data=MT%3AY3.13OTB00KA0648G00

Note: use the link that is provided in your uart log. The QR code contains the info the smartphone app needs for provisioning. 
 
9) copy your link to a web browser on your computer. The webpage should show a QR Code.
10) start the CHIPTool app on your Android phone. And click on "PROVISION CHIP DEVICE WITH THREAD".
11) The smartphone camera can now be used to scan the QR Code.
12) You should see a messeage "Scanning for BLE device 3840" on your smartphone. In case this message does not disappear, please press Button 4 on your light bulb (nrf52840DK).
13) If the previous step succeeded you see following Info on your smartphone:
       Enter credentials of the Thread
       network that you want to put your
       CHIP device on.
       
       Channel: 15
       PAN ID: 1234
       Extended PAN ID: 11:11:11:11:22:22:22:22
       Master Key: 00:11:22:33:44:55:66:77:88:99:AA:BB:CC:DD:EE:FF

Now you just have to click the button "SAVE NETWORK"

### Controlling the Matter Device (Light Bulb) with the Smartphone app
After provisioning successfully completed you can now control the "Light Bulb" via the Smartphone app:

14) click on "LIGHT ON/OFF & LEVEL CLUSTER" 
15) by pressing "ON" and "OFF" buttons you should see that LED 2 on your nRF52840DK turns on and off. You can also use the "TOGGLE" button. 
16) The "READ" button allows you to read the state of the LED on your board.
17) When you click "ON" then the slider can be used to dim the LED. 


[go to previous page](../README.md)
