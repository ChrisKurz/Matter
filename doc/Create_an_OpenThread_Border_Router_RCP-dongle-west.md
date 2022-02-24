# Create an RCP device (using nRF52840dongle and west tool)  [nRF Connect SDK V1.8.0 needed!!!]

1. Open *nRF Connect for Desktop* and start *Toolchain Manager*
2. As soon as a *nRF Connect SDK* version 1.8.0 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

__NOTE:__ Please use nRF Connect SDK Version 1.8.0. Some needed files are not available in nRF Connect SDK V1.9.0!!!

3. A command line window should open and you are pointed to the path where the *nRF Connect SDK* is installed. Go to path __nrf/samples/openthread/coprocessor__:

       cd nrf/samples/openthread/coprocessor
            
4. Build the project:

       west build -p always -b nrf52840dongle_nrf52840 -- -DOVERLAY_CONFIG="overlay-rcp.conf ../cli/overlay-thread_1_2.conf overlay-usb.conf" -DDTC_OVERLAY_FILE="usb.overlay"

5. Go to build directory:

       cd build

6. Prepare the RCP firmware image to be programmed via the nRF52840dongle's bootloader:

       nrfutil pkg generate --hw-version 52 --sd-req=0x00  --application zephyr/zephyr.hex --application-version 1 zephyr/zephyr.zip 
       
7. Connect the nRF52840dongle and press the "RESET" button (red LED should blink afterwards)
8. Start firmware update by entering following instruction:

       nrfutil dfu usb-serial -pkg zephyr/zephyr.zip -p COM11
       
__NOTE:__ please check with Windows Device Manager the COM port of the nRF52840dongle. In my case the COM port is COM11. You have to enter here the COM port used on your computer!



