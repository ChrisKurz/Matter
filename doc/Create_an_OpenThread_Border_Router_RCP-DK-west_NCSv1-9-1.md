# Create an RCP device (using nRF52840DK and west tool)  [nRF Connect SDK V1.9.1 needed!!!]

1. Open *nRF Connect for Desktop* and start *Toolchain Manager*
2. As soon as a *nRF Connect SDK* version 1.9.1 is installed you see the "Open IDE" buttons and for each installed version you also see a button with and arrow showing downwards. Click this button and click on "open command prompt" or "open bash".

3. A command line window should open and you are pointed to the path where the *nRF Connect SDK* is installed. Go to path __nrf/samples/openthread/coprocessor__:

       cd nrf/samples/openthread/coprocessor
            
4. Build the project:

       west build -p always -b nrf52840dk_nrf52840 -- -DOVERLAY_CONFIG="overlay-rcp.conf" -DCONFIG_OPENTHREAD_THREAD_VERSION_1_2=y

5. Flash the firmware image to the nRF52840DK:

       west flash -erase
