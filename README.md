# Matter

This document describes the set-up of a matter demo network. It is possible to setup different development environments. In this article we will focus on a setup with OpenTread Border Router and Matter controller on the same device, as shown in the following figure. 

![](img/OTBR_and_Matter_Controller_on_same_device.JPG)

More development environments are described here:
https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/ug_matter_configuring.html#

## Setup the Development Environment

In our setup we will use a Raspberry PI and run an OTBR and the Matter Controller on it. So let's start and prepare it. 

[First, create an OpenThread Border Router (OTBR) including Matter Controller](doc/Create_an_OpenThread_Border_Router.md)

Now we need Matter Accessories or Matter Devices in our setup. There are several examples within the nRF Connect SDK. Here is a description how to create the Matter Devices and add it to the network. 

- Matter Light bulb
- Matter Light switch
- [Matter Door lock](doc/Matter_Device_Door_Lock.md)
