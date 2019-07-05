# NØ SETUP

For NØ SCRIPT, we've setup a network so that we can remotely manage 12 Raspberry Pis. The idea is to go cold turkey in regards to moving away from the Graphic User Interface.

Using networked Raspberry Pis seemed like a reasonable approach to introducing students to computer networking principles. This page explains how the network is setup along with some basic concepts for understanding how it's working.

## Show me your ID

Computers have different numbers that identify who they are and where there are in the network. The combination is like an ID card that has a unique number assigned to an individual, and your address.

### MAC

Machine Access Control (MAC) is a unique number that is displayed as a grouping of 6 octets in hexadecimal. It looks like this:

`AB:CD:EF:01:23:45`

* Each octet is separated by either a `:` or `-`
* Octet is computer science speak for 8 bits or a byte (by eight)
* Hexadecimal is a base 16 numeric system. Decimal counts from `0-9`. Hexadecimal counts from `0-9, A-F`

These numbers are typically "burned" into network devices permanently, but like an ID, they can be "faked" or spoofed.

### IP ADDRESS

IP is short for Internet Protocol. The protocol describes how computers communicated to one another across network boundaries. It enables internet working at a local and global level enabling the Internet.

An IP address is assigned to your computer by the router when you connect to it using the Dynamic Host Configuration Protocol (DHCP). It does this by associating your device's MAC address with the IP address it assigns you.

The Address Resolution Protocol describes the discovery of MAC and IP address associations.

### NØSCRIPT WiFi

This is our network of Pis. Connect to NØSCRIPT in order to access your Pi wirelessly.

#### How it's connected (roughly)

```
Your Computer < wireless > Wifi Router
Your Raspberry Pi < ethernet > Ethernet Router
Ethernet Router < ethernet > Wifi Router
Wifi Router < ethernet > 4G Internet Gateway

Your Computer < via ssh > Your Raspberry Pi
```
 
* The internet connection is provided by a Netgear 4G gateway device connected to a wireless router's WAN port.
* An ethernet router acting as a switch is connected to the wireless router's Local Area Network (LAN) port via its WAN port.
* A smaller, 5-port switch is connected the ethernet router via the LAN ports.
* The Raspberry Pis are connected to the ethernet router and switch LAN ports via their ethernet ports.
* When you connect to the Wifi network, NOSCRIPT, you are able to connect to you Raspberry Pi via Secure Shell (`ssh`).

#### Class MAC and IP Address Bindings

Each Raspberry Pi's MAC address has been manually assigned a permanent IP address. Each time they connect to the network, the router will assign it the same IP address.

```
B8-27-EB-EE-FD-8A	192.168.1.101		
B8-27-EB-74-DD-BB	192.168.1.102		
B8-27-EB-F6-23-4B	192.168.1.103		
B8-27-EB-75-79-C2	192.168.1.104		
B8-27-EB-A0-23-FA	192.168.1.105		
B8-27-EB-51-0A-9F	192.168.1.106		
B8-27-EB-FF-FE-BB	192.168.1.107		
B8-27-EB-67-6A-43	192.168.1.108		
B8-27-EB-F4-CC-9E	192.168.1.109		
B8-27-EB-11-8D-94	192.168.1.110		
B8-27-EB-73-D0-E6	192.168.1.111		
B8-27-EB-9D-0F-53	192.168.1.112		
```