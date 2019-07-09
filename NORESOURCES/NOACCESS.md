# NØ ACCESS

Configuring your Raspberry Pi to act as a wireless access point and serve up a captive portal.

## Agenda


* Install tools
* Distribute WiFi dongles
* Configure tools
* Launch AP
* Create serve up captive portal

### Requirements:

* `hostapd` - used to create a wireless access point. 
* `dnsmasq` - a lightweight framework for providing network infrastructure, namely Domain Name Service (DNS), and Dynamic Host Configuration Protocol (DHCP).
* `nginx` - a lightweight web server 

*resources:*

* [Setting up a Raspberry Pi as an access point in a standalone network (NAT)](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md)
* [Raspberry Pi Access Point and Captive Portal](https://brennanhm.ca/knowledgebase/2016/10/raspberry-pi-access-point-and-captive-portal-without-internet/)

## Install Tools:

### 1. packages

To save a bit of time, we'll update the apt-get package list, skip the upgrade, and install `hotapd` and `dnsmasq`. Run the following commands to do this:


```
$ sudo apt-get update
$ sudo apt-get install hostapd dnsmasq nginx
```

### 2. Hardware


The PAU05 is a usb wifi dongle that supports assess point and monitor mode. Not all adapters have this capability and is something that requires a bit of additional research when shopping for other (more powerful) adapters.

Before plugging the dongle in, run:

```
$ ip a
```

This list the available network devices. `wlan0` is the current wireless interface. Anyways, plug this dongle into any of the USB ports. Run `$ ip a` again and you'll find there's a new device: `wlan1`. 

Write down the MAC address. You can find is after "link/ether", for example:

```
link/ether 9c:ef:d5:fc:30:1a
```

### 3. Predictable Network Interface Names

Depending on the boot sequence, the dongle and onboard wireless adapters might switch unpredictably between `wlan0` and `wlan1`. To assign a stable network device name to the dongle, we need to enable Predictable Network Interface Names:

1. run `$ sudo raspi-config`
2. select option 2, "Network Options" and press enter
3. select option N3, "Network interface names" and press enter
4. select "Yes" and press enter
5. Reboot

Now when you log back in and run `$ ip a`, we should see `wlan0`, and an interface with a unique id. My wifi dongle's name is now `wlx9cefd5fc3019`. Check your MAC address to confirm. The unique ID is based on the device's MAC.

Make sure to write this down for the next steps.

### 4. Create a hostname for hotspot network

When a host connects to our access point, it's nice if the IP address resolves to a host name. To configure this we need to edit the `/etc/hosts` file:

```
$ sudo nano /etc/hosts
```

and add the following line

```
192.168.42.1	localnet
```

The name can be anything we want and you can change this in the future.

## Configure Tools:

### 1. Stop `hostapd` & `dnsmasq` services

First, let's check the status of the services we just installed: `hostapd` and `dnsmasq`. We can check the status of a service by using the `systemctl` command with the `status` option:

```
$ sudo systemctl status hostapd
```

If you get the output:

```
● hostapd.service
	Loaded: masked (Reason: Unit hostapd.service is masked.)
	Active: inactive (dead)
```

run:

```
$ sudo systemctl unmask hostapd
$ sudo systemctl enable hostapd
```

Checking the status again should give us:

```
● hostapd.service - Advanced IEEE 802.11 AP and IEEE 802.1X/WPA/WPA2/EAP Authenticator
	Loaded: loaded (/lib/systemd/system/hostapd.service; enabled; vendor preset: enabled)
	Active: inactive (dead)
```

This is what we want.

If `status` is "active", run:

```
$ sudo systemctl stop hostapd
```

Then run:

```
$ sudo systemctl stop dnsmasq
```

Both `hostapd` and `dnsmasq` should be stopped. You can check their 

### 2. Modify DHCP settings

Setup a static IP for the access point we'll run on our USB wifi dongle.

```
$ sudo nano /etc/dhcpcd.conf
```

Add the following lines:

```
interface <your_assigned_network_device_name>
	static ip_address=192.168.42.1/24
	nohook wpa_supplicant
```

Save and exit.

Restart the `dhcpcd` service to apply the changes.

```
$ sudo systemctl restart dhcpcd
```

### 3. Configure `hostapd`

`hostapd` is a lightweight service that runs in the background (daemonized) and makes the Pi work like a WiFi access point. We need to give it some basic settings like name (ssid), interface, and any security settings. We're configuring an open WiFi access point. I've left WPA settings commented out so you can lock things down if you want.

#### Create a configuration file:

```
$ sudo nano /etc/hostapd/hostapd.conf
```

Add to it:

```

# Network Name
ssid="NØ PORTAL"

# Network Region
country_code=FR

# Network Device Settings
interface=<your_assigned_network_device_name>                                  
driver=nl80211
hw_mode=g
channel=6
ieee80211n=1
wmm_enabled=0
macaddr_acl=0 
ignore_broadcast_ssid=0

#If you want to set a password for network access
#auth_algs=1
#wpa=2
#wpa_key_mgmt=WPA-PSK 
#wpa_pairwise=TKIP
#rsn_pairwise=CCMP
#wpa_passphrase=NOSECRET2019
```

Be sure to change `<your_assigned_network_device_name>` to the predictable network name your Pi gave the WiFi adapter.

### 4. Tell `hostapd` where to find the configuration file:

#### Create & edit `/etc/default/hostapd`

```
$ sudo nano /etc/default/hostapd
```

Find the line that reads:

```
#DAEMON_CONF=""
```

uncomment and change to:

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

#### Edit `/etc/init.d/hostapd`


```
$ sudo nano /etc/init.d/hostapd
```

find

```
DAEMON_CONF=
```

change to

```
DAEMON_CONF=/etc/hostapd/hostapd.conf
```

### 5. Configure `dnsmasq`

Make a copy of the original configuration file:

```
$ sudo cp /etc/dnsmasq.conf{,.bak}
```

Edit the confinguration file:

```
$ sudo nano /etc/dnsmasq.conf
```
Add:

```
# Never forward addresses in the non-routed address spaces.
bogus-priv
#　Add other name servers here, with domain specs if they are for　non-public domains.
server=/localnet/192.168.42.1
# Add local-only domains here, queries in these domains are answered　from /etc/hosts or DHCP only.
local=/localnet/
# Make all host names resolve to the Raspberry Pi's IP address
address=/#/192.168.42.1
# Specify the interface that will listen for DHCP and DNS requests
interface=<your_assigned_network_device_name>
# Set the domain for dnsmasq
domain=localnet
# Specify the range of IP addresses the DHCP server will lease out to devices, and the duration of the lease
dhcp-range=192.168.42.5,192.168.42.250,2h
# Specify the default route
dhcp-option=3,192.168.42.1
# Specify the DNS server address
dhcp-option=6,192.168.42.1
# Set the DHCP server to authoritative mode.
dhcp-authoritative
```
Be sure to change `<your_assigned_network_device_name>` to the predictable network name your Pi gave the WiFi adapter.

### 6. Start `hostapd` and `dnsmasq`

#### `hostapd`

There's a good chance `hostapd`, may be masked, requiring us to explicitly start it up as follows:

```
$ sudo systemctl start hostapd
```

This might fail, but it could be because of other system services running. We'll reboot after starting dnsmasq, then check the service status.

#### `dnsmasq`

```
$ sudo systemctl start dnsmasq
```
<!-- 

Might have to revise dnsmasq configuration

Jul 09 16:28:19 raspberrypi dnsmasq[1074]: compile time options: IPv6 GNU-getopt DBus i18n IDN DHCP DHCPv6 no-Lua TFTP conntrack i
Jul 09 16:28:19 raspberrypi dnsmasq-dhcp[1074]: DHCP, IP range 192.168.42.5 -- 192.168.42.250, lease time 2h
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: using local addresses only for domain localnet
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: ignoring nameserver 192.168.42.1 - local interface
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: reading /run/dnsmasq/resolv.conf
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: using local addresses only for domain localnet
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: using nameserver 192.168.1.1#53
Jul 09 16:28:19 raspberrypi dnsmasq[1074]: read /etc/hosts - 6 addresses
Jul 09 16:28:19 raspberrypi dnsmasq[1075]: Too few arguments.
Jul 09 16:28:19 raspberrypi systemd[1]: Started dnsmasq - A lightweight DHCP and caching DNS server.

-->

### 7. Create a basic portal page

<!--
Create a directory to put the portal page files in:

```
$ sudo mkdir /usr/share/nginx/html/captive-portal --mode=u+rwx,g+srw,o-w
```

Change the owner to `nostudent:www-data` to allow for sudo-less editing

```
$ sudo chown nostudent:www-data -R /usr/share/nginx/html
```
-->

Make a backup of the default `nginx` `index.html` page:

```
sudo cp /usr/share/nginx/html/index.html{,.bak}
```

Create the portal page (draft) in a fresh `index.html` page:

```
$ echo "<h1>Hello</h1>" > /usr/share/nginx/html/index.html
```
<!--
Connect to your access point and navigate to `test.com`. You should be redirected to the default `nginx` page.
-->

### 8. Configure `nginx`

Create a configuration file for the captive portal.

```
$ sudo nano /etc/nginx/sites-available/captive-portal.conf
```

Copy this basic configuration into it:

```
server {
	# Listen for requests over both HTTP and HTTPS
	listen 80;
	listen [::]:80;
	#listen 443 ssl;
	#listen [::]:443;
	# Present a friendly name to the client, instead of an IP address
	server_name localnet;
	#Include HTTPS configuration from the snippets directory
	#include snippets/self-signed.conf;
	#include snippets/ssl-params.conf;
	
	root /usr/share/nginx/html;

	index index.html index.htm index.nginx-debian.html;

	# Redirect requests for /generate_204 to open the captive portal screen
	location /generate_204 {
		return 302 http://localnet/index.html;
	}

	# Redirect requests for files that don't exist to the captive-portal page
	location / {
		try_files $uri $uri/ /index.html;
	}
}
```

Create a symbolic link to the conf file:

```
$ sudo ln -s /etc/nginx/sites-available/captive-portal.conf /etc/nginx/sites-enabled/captive-portal.conf
```

Remove the symbolic link to the default from the `sites-enabled` folder:

```
$ sudo rm /etc/nginx/sites-enabled/default
```

We're now ready to reboot and see if it worked!

```
$ sudo reboot
```

### 9. Connect

Connect to your open access point and see if your captive portal page pops up automatically.