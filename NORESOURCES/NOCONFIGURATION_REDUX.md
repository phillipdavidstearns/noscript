# NØ CONFIGURATION REDUX

Configure your Raspberry Pi to automatically connect to a WiFi access point and request a static IP.

## Outline:

* Edit wpa_supplicant.conf
* Edit dhcpcd.conf
* Reboot

## Configure `wpa_supplicant` and `dhcpcd`

### Log into your Pi

```
$ ssh nostudent@<your_pi_IP>
```

### Edit `wpa_supplicant.conf`

Change your working directory:

```
$ cd /etc/wpa_supplicant
```

Make a backup of the `wpa_supplicant.conf` file:

```
$ sudo cp wpa_suplicant.conf{,.bak} 
```

Edit the file with `nano`:

```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add the following:

```
network={
  ssid=NOSCRIPT
  psk=NOHACKING2019
}
```

For more on doing this in more secure ways [go here](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

## Configure `dhcpcd`

* Make a backup by with `sudo cp /etc/dhcpcd.conf{,.bak}`
* Edit the `dhcpcd.conf` file by adding the following text just below the section `# Example static IP configuration:`

```
interface wlan0
static ip_address=your.hotspot.subnet.100/24
```

Note: If you're using a USB WiFi dongle, enable "predictable network names' using `sudo raspi-config` and reboot. The USB dongle will then be assigned a unique name based on it's MAC address. For instance, if the MAC address is `AB:CD:EF:01:23:45` then your predictable device name will be `wlxabcdef012345`. The built in wireless device will then be named `wlan0`.

Reboot!

If everything goes according to plan, you should be able to 