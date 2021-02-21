# OpenWrt -  Using O2 6431 Box 

## Introduction
Here i will be working on the project openwrt in which, i will change the router original firmware to OpenWrt, i will show the steps needed without using serial connection, just only using the network cable. The router has a DSL port, and 4 Network ports, i will configure the router to use LAN1 to connect to Internet through another router, in addition to creating a rescue configuration using LAN4, that will be connected to a VPN server for telephony and gaming purposes. For a full telephony experience, we will install asterisk and configure to use the telephony port.

## Table Of Contents

<!-- toc -->

- [Flashing](#Flashing)
- [Resuce Port](#Create-a-Resuce-Port)
- [Connect Internet with LAN1](#Connect-Internet-through-LAN-1)

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
Up till now not all the available flash area is available, because some is used by old configurations. We will use it in this section.

- download the OpenWrt Upgrade Image, this is the NOR Image
http://downloads.openwrt.org/releases/19.07.7/targets/lantiq/xrx200/openwrt-19.07.7-lantiq-xrx200-arcadyan_vgv7510kw22-nor-squashfs-sysupgrade.bin

- Open http://192.168.1.1 and in the LUA OpenWrt page, goto Flash firmware under 
   System -> Backup / Flash Firmware
   http://192.168.1.1/cgi-bin/luci/admin/system/flash

- Select the NOR image and click ok

- Warning ! Do this step on your own responsibility, for me it worked, i am not responsible for any issues that might result.

- The router will give a warning that this is not the correct image. It will give an option box to force uploading the firmware. Click OK

- Ping the router and give it time to reboot, wait about 6 min.

- now you have used the complete flash area, to check, try to open the secret page, it will not be available. Try step 1, it will not be available.


## Create a Resuce Port

I will use LAN4 for a rescue function with custom router ip of 10.10.10.10, so that if i lose connection to the router, i can setup a static ip of my pc to 10.10.10.15 and connect to the router but it should give you a dynamic ip in this range.
	
	In Network-> Switch 
	-------------------
		click on Add VLAN to a add a new VLAN zone
		Set LAN 4 to be untaged in VLAN ID 3 and off in VLAN ID 1
		Set CPU eth0 to be taged in VLAN ID 3
		Click on Save and Apply

	In Network-> Interface 
	----------------------
		click on Add new interface
		New Interface name: Rescue
		Protocol: Static address
		Interface: in custom search for eth0.3
		Click Save, and this will open a next configuration window for Resuce Interface
	
	General Settings:
	--------------------	
		IPv4 address: 10.10.10.10
		IPv4 netmask: 255.255.255.0
		Ipv6 assignment length: 60
	
	Advanced Settings:
	------------------
		Use buildin IPv6-management: yes
		Force link: yes
	
	Physical Settings:
	------------------
		Check Interface is set to eth0.3
	
	Firewall Settings:
	------------------
		Firewall zone: select Lan
	
	DHCP Server
	-----------
	click on Setup DHCP Server
		
		General Setup:
		--------------
		Start: 100
		Limit: 150
		Lease time: 12h
		
		Advanced Settings:
		------------------
		Dynamic DHCP: yes
		
		IPv6 Settings:
	    --------------
	    Router Advertisment-Service: server mode
	    DHCPv6-Service: server mode
	    NDP-Proxy: disabled
	    DHCPv6-Mode: statesss+stateful 
	    
	    Save and Apply

Now when i connect to port 4, i get ip 10.10.10.133 and can ping 10.10.10.10 sucessfully
Also i can connect to the router under 192.168.1.1


## Connect Internet through LAN 1
Now i will configure LAN1 port to be used as Internet source connected to a second router.

- In Network-> Switch 
		Create a new VLAN using Add VLAN, this will add VLAN Id 4
		Set CPU(eth0) in VLAN ID 4 to taged
		Set LAN1 in VLAN ID1 to off (Remove LAN1 from VLAN ID1)
		Set LAN1 in VLAN ID4 to untaged
		Save and apply
	

- In the Network->Interfaces
	Click on add a new interface, this will open an interface window
	
		Name: WAN2
		Protocol: DHCP client
		Bridge: unchecked
		Interface: eth0.4
		
		Click on Create Interface: This will open Interfaces >> WAN2 window
		
			General Settings:
				HostName: InternetPort
				Protocol: DHCP Setting
			Physical Settings:
				Bridge interfaces: yes			 
				Interface: eth0.4 and search for radio0.network1
			Firewall Settings:
				Zone: Wan
	
	Save and apply, disconnect the network on LAN1 and Connect LAN1 port to the internet
	
	now you will have an internet connection over LAN1



	
	
	



	    
	