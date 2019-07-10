# NØ SCRIPT @ [NØ SCHOOL NEVERS 2019](https://noschoolnevers.com)

![](images/screen_003.gif)

A collection of modules and walkthroughs for helping you get the most (fun) out of your Raspberry Pi.

Grab the goods:

```
git clone https://github.com/phillipdavidstearns/noscript.git
```
## [NØ RESOURCES](NORESOURCES/)

* [NØ CONNECTION](NORESOURCES/NOCONNECTION.md) - Connecting to our Pis over Secure Shell (ssh)
* [NØ CONFIGURATION](NORESOURCES/NOCONFIGURATION.md) - Learning bash by Configuring Raspberry Pis to connect to a personal Wifi Hotspot
	* [NØ CONFIGURATION (REDUX)](NORESOURCES/NOCONFIGURATION_REDUX.md) - Configuring Raspberry Pis to automatically connect to a WiFi access point
* [NØ PIPING](NORESOURCES/NOPIPING.md) - Piping and Redirection using fun hackery tools
* [NØ DATA FILTERING](NORESOURCES/NODATAFILTERING.md) - Data pre-visualisation and filtering
* [NØ DATA CONVERSION](NORESOURCES/NODATACONVERSION.md) - Transforming information on the command line
* [NØ HOSTING](NORESOURCES/NOHOSTING.md) - Host an HTTP Server (webpage) on your Pi
* [NØ ACCESS](NORESOURCES/NOACCESS.md) - Setting up the Raspberry Pi to act as a WiFi access point and serve up a captive portal.

A class by [Jerome Saint-Claire](https://www.saint-clair.net/) and [Phillip David Stearns](https://phillipstearns.com)


## NØ USE

What are these modules good for? Learning Raspberry Pi mostly. What's a Raspberry Pi good for? Having the power of a small computer in the footprint of a pack of playing cards (roughly). It's paacked with 4x USB ports, HDMI, analog stereo audio out, GPIO pins for interfacing with hardware, WiFi, Bluetooth, and runs off 5 volts.

### NØ CONNECTION

Connecting to our Pis over Secure Shell (ssh)

This module is an essential step away from relying on graphical user interfaces (GUI) to control a computer. On networked systems, you might not have direct physical access to a computer (no monitor, no mouse), but with `ssh`, you can connect using the command line and control it like it was right in front of you.

If you've configured your Raspberry Pi as a WiFi access point, you can connect directly to it over WiFi and `ssh` into it without the need for cables.

### NØ CONFIGURATION

Learning bash by Configuring Raspberry Pis to connect to a personal Wifi Hotspot.

`bash` is a common text based way to interact with a computer via the command line interface. It's a bit intimidating to jump into if we're used to mousing about and clicking our way through GUIs.

This module walks students through the basics of `bash` by taking them through process of connecting their Pi to their own hotspot so that it can be controlled wirelessly on the go! The current network configuration has all the Pis physically connected via ethernet cables.

### NØ CONFIGURATION (REDUX)

Configuring Raspberry Pis to automatically connect to a WiFi access point and request a static IP.

A simplified, more direct version of the walkthrough above.

### NØ PIPING

Piping and Redirection using fun hackery tools.

A module that demonstrates a crucial process of piping and redirecting output from commands. This capability allows us to do fun things with text and output the to text files. w00t!

### NØ DATA FILTERING

Data pre-visualization and filtering for when you have to work with massive data sets or find important items in long lists.

### NØ DATA CONVERSION

Transforming information on the command line for further processing.

### NØ HOSTING

Host an HTTP Server (webpage) on your Pi so you can do web development or share files.


### NØ ACCESS

Setting up the Raspberry Pi to act as a WiFi access point and serve up a captive portal creates the possibility for local WiFi publishing, WiFi dead drops, mobile net art galleries, etc...