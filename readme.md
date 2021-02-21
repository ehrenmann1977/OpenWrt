# OpenWrt -  Using O2 6431 Box 

## Introduction
Here i will be working on the project openwrt in which, i will change the router original firmware to OpenWrt, i will show the steps needed without using serial connection, just only using the network cable. The router has a DSL port, and 4 Network ports, i will configure the router to use LAN1 to connect to Internet through another router, in addition to creating a rescue configuration using LAN4, that will be connected to a VPN server for telephony and gaming purposes. For a full telephony experience, we will install asterisk and configure to use the telephony port.

## Table Of Contents
<!-- toc -->

- [Flashing] (#Flashing)
- [Resuce Port] (#Create a Resuce Port)

<!-- tocstop -->


## Flashing 
In the following webpage, there is the firmware that should be used.
https://openwrt.org/toh/arcadyan/vgv7510kw22

1- Step 1 - Flash a working openwrt 

- download the OpenWrt Install Image, this is the last available image, it is the brn image
http://downloads.openwrt.org/releases/19.07.7/targets/lantiq/xrx200/openwrt-19.07.7-lantiq-xrx200-arcadyan_vgv7510kw22-brn-squashfs-factory.bin

- Restart the router and while booting, press on the reset button, you need a small needle or pen tip

- When the router starts, it will enable the recovery web interface at page http://192.168.1.1

- Go to this page, select in Upgrate Target : Firmware, choose the BRN File which you downloaded before.

- open a terminal and ping 192.168.1.1 -t to see when the router reboots, it does not ping while flashing

- After reboot, you will have an openwrt working firmware

- Now we will still have the original boot loader of the router, to check it or to revert use the following secret page. Till now there not the complete flash space is used.
http://192.168.1.1/undoc_upgrade.stm

2- Step 2: Use complete available Flash area

- download the OpenWrt Upgrade Image, this is the NOR Image
http://downloads.openwrt.org/releases/19.07.7/targets/lantiq/xrx200/openwrt-19.07.7-lantiq-xrx200-arcadyan_vgv7510kw22-nor-squashfs-sysupgrade.bin

- Open http://192.168.1.1 and in the LUA OpenWrt page, goto Flash firmware under 
   System -> Backup / Flash Firmware

- Select the NOR image and click ok

- Warning ! Do this step on your own responsibility, for me it worked, i am not responsible for any issues that might result.

- The router will give a warning that this is not the correct image. It will give an option box to force uploading the firmware. Click OK

- Ping the router and give it time to reboot, wait about 6 min.

- now you have used the complete flash area, to check, try to open the secret page, it will not be available. Try step 1, it will not be available.


## Create a Resuce Port
