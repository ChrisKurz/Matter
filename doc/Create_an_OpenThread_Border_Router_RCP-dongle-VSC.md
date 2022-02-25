# Create an RCP device (using nRF52840dongle and Visual Studio Code)  [nRF Connect SDK V1.8.0 needed!!!]

1. Open *nRF Connect for Desktop* and start *Toolchain Manager*
2. install *nRF Connect SDK version 1.8.0* and start Visual Studio Code (press "Open VS Code" button).

__NOTE:__ Please use nRF Connect SDK Version 1.8.0. Some needed files are not available in nRF Connect SDK V1.9.0 or later!!!

3. Open the Coprocessor project. Here is the path of this project:  __nrf/samples/openthread/coprocessor__

4. "Add Build Configuration" for this project. Following settings should be used:
- Board:  nrf52840dongle_nrf52840
- Configuration:  prj.conf
- Kconfig fragments:  no entry here
- Extra CMake arguments:  please enter here the following line

      -DOVERLAY_CONFIG="overlay-rcp.conf;../cli/overlay-thread_1_2.conf;overlay-usb.conf" -DDTC_OVERLAY_FILE="usb.overlay"

5. Build the project by clicking on "Pristine build"

6. Open cammnad line tool:
- Open nRF Connect for Desktop and start Toolchain Manager
- As soon as a nRF Connect SDK version 1.8.0 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

7. Go to the coprocessor project's build directory

8. Prepare the RCP firmware image to be programmed via the nRF52840dongle's bootloader:

       nrfutil pkg generate --hw-version 52 --sd-req=0x00  --application zephyr/zephyr.hex --application-version 1 zephyr/zephyr.zip 
       
9. Connect the nRF52840dongle and press the "RESET" button (red LED should blink afterwards)
10. Start firmware update by entering following instruction:

        nrfutil dfu usb-serial -pkg zephyr/zephyr.zip -p COM11
       
__NOTE:__ please check with Windows Device Manager the COM port of the nRF52840dongle. In my case the COM port is COM11. You have to enter here the COM port used on your computer!
