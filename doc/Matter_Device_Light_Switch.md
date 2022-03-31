# Example Matter Device: Light Switch

The sample description can be found here:

https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/samples/matter/light_switch/README.html

## Realize Light Switch on an nRF52840DK

This description is using nRF Connect SDK V1.8.0. Note that later versions may differ from the example delivered with this SDK version!

The quickest way to setup the board is to use command line instructions. Here is the description:

1. Open nRF Connect for Desktop and start Toolchain Manager

2. As soon as a nRF Connect SDK version 1.8.0 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

NOTE: Please use nRF Connect SDK Version 1.8.0. 

3. go to the folder __nrf/samples/matter/light_switch__

       cd nrf/samples/matter/light_switch

4. in case there is a build folder, remove it.

       rm -rf build
       
5. build the project:

       west build -b nrf52840dk_nrf52840

6. connect the nRF52840DK development board to the computer andfalsh the firmware to it:

       west flash --erase

7. Open a terminal program (e.g. Putty) and see logs from the development board. Default settings for terminal program: 115200 Bd, 8 databits, 1 stop bit, no parity


[go to previous page](../README.md)

