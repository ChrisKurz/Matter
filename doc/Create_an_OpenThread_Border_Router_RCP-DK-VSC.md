# Create an RCP device (using nRF52840DK and Visual Studio Code)  [nRF Connect SDK V1.8.0 needed!!!]

1. Open *nRF Connect for Desktop* and start *Toolchain Manager*
2. install *nRF Connect SDK version 1.8.0* and start Visual Studio Code (press "Open VS Code" button).

__NOTE:__ Please use nRF Connect SDK Version 1.8.0. Some needed files are not available in nRF Connect SDK V1.9.0 or later!!!

3. Open the Coprocessor project. Here is the path of this project:  __nrf/samples/openthread/coprocessor__

4. "Add Build Configuration" for this project. Following settings should be used:
- Board:  nrf52840dk_nrf52840
- Configuration:  prj.conf
- Kconfig fragments:  no entry here
- Extra CMake arguments:  please enter here the following line

      -DOVERLAY_CONFIG="overlay-rcp.conf;../cli/overlay-thread_1_2.conf"

5. Build the project by clicking on "Pristine build"

6. Download the firmware to the board by clicking in Visual Studio Code's nRF Connect Action menu on "Flash".
