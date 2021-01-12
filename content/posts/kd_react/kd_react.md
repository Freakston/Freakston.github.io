+++
title = "Setup Kernel Debugging of ReactOS using PUTTY/ROSDBG"
date = "2020-04-06"
author = "Freakston"
tags = ["Development","ReactOS"]
keywords = ["ReactOS"]
description = "Kernel Debugging for ReactOS"
showFullContent = false
+++

## Setup Kernel Debugging for ReactOS -

In this post I will be guiding you guys on how to set up a kernel debugger for ReactOS. ReactOS can be built using the MSVC or GCC. In this post we will be using a ReactOS built using MSVC.

For official reference you can refer the ReactOS [wiki](https://reactos.org/wiki/Welcome_to_the_ReactOS_Development_Wiki).

## Tools needed

1. [com0com](https://code.google.com/archive/p/powersdr-iq/downloads)
2. [Putty](https://www.putty.org/)
3. Virtual Box

## Steps

1. Install com0com with the following components selected.

![Components_to_install](Components_to_install.png)

2. Make sure the settings page has the following options checked.

![setup com0com](setup_com0com_reactos.png)

3. Setup Virtualbox serial ports as follows

![vbox_serial_port](vbox_serial_port.png)

4. Now we can connect to the port COM4 in RosDBG or putty and we have a kd.
![putty_reactos](putty_reactos.png)

## References 
- https://reactos.org/wiki/Com0com  
- https://reactos.org/wiki/Debugging
- https://reactos.org/wiki/reactos_Remote_Debugger