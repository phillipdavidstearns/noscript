# NØ SCRIPT

Computer Networks as Medium

A survey of technical aspects of computer networking. Some Humor and fun things sprinkled throughout.

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

#### How it's connected

An internet connection is provided by a Netgear 4G gateway device.
This device provides a Wide Area Network (WAN) connection.
A wireless router is connected to the gateway via its WAN port.
An ethernet router acting as a switch connected to the wireless router's Local Area Network (LAN) port via its WAN port.
A smaller, 5-port switch is connected the ethernet router via their LAN ports.
The Raspberry Pis are connected to the ethernet router and switch LAN ports via their ethernet ports.

Each Raspberry Pi's MAC address has been manually assigned a permanent IP address. Each time they connect to the network, the router will assign it the same IP address.

#### Class MAC and IP Address Bindings

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

## Thursday

* Review
* Configure `wpa_supplicant`
* Folder navigation
* `pwd`
* `ls`
* `cd`
* `cp` - copy
* `nano` - text editor
* Configure `dhcpcd`
* `sudo`
* `reboot`

* `figlet` & `toilet`
* `apt`, `apt-get`
* `<command> --help`, `command -h`
* `man <command>`
* piping

## Review

### `bash` - Bourne Again Shell

Bash is a shell that provides a text based interface to execute operating system commands.

To run a command, like `ssh`, we type the command and press enter.

```
nostudent@192.168.0.10X:~$ ssh
```

Output: 

```
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command]
```

Ooops! `ssh` wants some additional options and/or arguments. If we're just starting out, this usage message is a bit cryptic. This is where the `man` command comes in handy.

### `man` - Manual

The manual command opens the manual for a desired command. Type `man` followed by the command and press enter. For instance, what if we want to learn more (in less) about the manual command itself?

```
nostudent@192.168.0.10X:~$ man man
```

This is the manual for `man`. :)

#### How to use the manual:

```
SYNOPSIS
       man  [-acdfFhkKtwW]  [--path]  [-m system] [-p string] [-C config_file]
       [-M pathlist] [-P pager] [-B browser] [-H htmlpager] [-S  section_list]
       [section] name ...
```

**Basic Navigation**

* Search `/<search_term>` - example: `/command`
	* `n` - next match
	* `b` - previous match (back)
	* up & down arrows - navigate previously entered search terms
	* *advanced users* useregular expressions to search
* `-i` - press this and enter to toggle case sensitivity
* `G` - jump to END
* `gg` - jump to beginning
* `h` - The *whole story* about how to use the less command to navigate the manual
* `q` - quit!

So we did the complicated way first... but what if there's no manual (or if you're a little more 1337)?

* `<command> --help`, `command -h`

This will typically bring up a usage summary as with the `ssh` example we started with.

### `ssh` - Secure shell

Allows for an encrypted remote terminal access to a computer. How we've been using `ssh` so far:

`ssh <user>@<host>`

Where `<user>` is `nostudent` and `<host>` is the ip address of your Pi.

##  Configure `wpa_supplicant`

We're going to try to lighten the load on the router and show you how you can connect to your Pi at home (or anywhere?).

### Outline:

1. Turn on hotspot access point
2. Connect your laptop to the access point
3. Find the ip address of your access point
4. Disconnect from your hotspot
5. Connect to NOSCRIPT wifi access point 
6. Log into your Pi
7. Create a backup of current wpa_supplicant.conf
8. Edit wpa_supplicant.conf
9. Edit dhcpcd.conf
10. reboot (and cross your fingers!)

### Activate hotspot

Cool

### Connect to hotspot

You got this

### Find your IP address

We need to know the subnet of our hotspot so that we can assign a "permanent" IP address to the Pi

Write it down. We'll need it later.

#### On Mac

* Click the Apple Icon
* Click System Preferences
* Click the 'Network' Icon

#### On PC

* Click the Wifi Icon
* Click Properties
* Scroll down until you find it

### Disconnect

### Connect to NOSCRIPT

* key: NOHACKING2019

### Log into your Pi

* `ssh nostudent@<your_pi_IP>`

## Editing `wpa_supplicant.conf`

What is `wpa_supplicant` and why do we need to configure it?

It's a utility that manages secure (and also insecure) Wifi connections. We're going to change the configuration file to tell the Pi to automatically connect to your hotspot.

But first we need to know where it is and how to get to it.

### Folder navigation

#### `pwd` - print working directory

Files are organized hierarchically in directories. `pwd` tells us exactly where we are, where the commands we execute will look for or use files here.

```
nostudent@192.168.0.10X:~$ pwd
/home/nostudent
```

The working directory is the point of reference for relative file paths.

* `/` - root directory
* `/home` - directory that contains directories of each user
* `/home/nostudent` - contains user directories and files

#### `ls` - list

Lists the contents of a directory. Try running:

```
nostudent@192.168.0.10X:~$ ls
```

* `la -l` - outputs list in a single column 
* `ls -a` - lists everything (including hidden files)
* `ls -h` - lists human readable file sizes

Try:

```
nostudent@192.168.0.10X:~$ ls -ahl
```

We've been listing the contents of our working directory, but we can also list the contents of other directories on our Pi. Try:

```
nostudent@192.168.0.10X:~$ ls -ahl /etc/wpa_supplicant/
```

This is where the wpa_supplicant.conf file is located. Have a look at the sequence of letters and dashes on the left side of the list. These are file permissions represented in a group of 10 characters, for example: `drwxr-xr-x`.

The first character `d` is whether the item is a directory.

The following sequence is grouped into three sections and indicate root, group, and user permissions.

* root has `rwx`
* group has `r-x`
* user has `r-x`

* `r` - read access enabled
* `w` - write access enabled
* `x` - execution enabled
* `-` - access disabled

The `wpa_supplicant.conf` permissions are set so that only root user has the ability to write to the file. But before we edit anything, we should make a back up. 

#### `cd` - change working directory

We don't really need to change our working directory to the location of the file to edit it, we can provide the complete path, i.e. `/etc/wpa_supplicant/wpa_supplicant.conf`, but that's a lot of typing.

Run:

```
nostudent@192.168.0.10X:~$ cd /etc/wpa_supplicant
```

#### `cp` - copy

Making a backup of a system configuration file is ALWAYS a good idea. `cp` takes two arguments, the source file to be copied and the destination file.

```
nostudent@192.168.0.10X:~$ cp wpa_suplicant.conf wpa_suplicant.conf.bak 
```

Permission denied? Only the root user can write to this directory! Use `sudo` to temporarily escalate your privileges to root.

```
nostudent@192.168.0.10X:~$ sudo cp wpa_suplicant.conf wpa_suplicant.conf.bak
```

Now we're ready to edit.

#### `nano` - text editor

Nano is a text editor.

`control + o` - works like save (can change file name too)
`control + x` - exit, gives option to save any changes)
`control + k` - cuts text
`control + u` - uncuts text
`control + g` - displays the help file for more commands

What we need to add:

```
network={
    ssid="<your_hotspot_name>"
    psk="<your_hotspot_password>"
}
```

For more on doing this in more secure ways [go here](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

## Configure `dhcpcd`

1. Locate the `dhcpcd.conf` file. It should be at `/etc/dhcpcd.conf`. Check out the `/etc` contents with `ls -alh`. 
2. Make a backup by copying `/etc/dhcpcd.conf` to `/etc/dhcpcd.conf.bak`. A shortcut `sudo cp /etc/dhcpcd.conf{,.bak}`
3. Edit the `dhcpcd.conf` file by adding the following text just below the section `# Example static IP configuration:`

```
interface wlan0
static ip_address=192.168.0.100/24
```

4. Save - `control + x`, Y, enter
5. `reboot` - `sudo reboot`

Let me know when you've rebooted, and which IP address your Pi is @.
