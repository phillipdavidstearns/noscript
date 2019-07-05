# NØ CONFIGURATION

Last session brought up some important issues concerning network load. In order to take some of the load off the one wifi, router, we'll try setting up our Pi's so that we can access them over our personal mobile hotspots.

## Agenda

* Review
* `<command> --help`, `command -h`
* `man <command>`
* Configure `wpa_supplicant`
* Folder navigation
* `pwd`, `ls`, `cd`, `cp`, `nano`, `sudo`
* Configure `dhcpcd`
* `reboot`, `apt`, `apt-get`, `figlet`, `toilet`, etc
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

### 1. Activate hotspot

Cool

### 2. Connect to hotspot

You got this

### 3. Find your IP address

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

### 4. Disconnect from hotspot

### 5. Connect to NOSCRIPT

Ask for the Key!

### 6. Log into your Pi

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

#### `ls` - list, `mkdir` - make directory, `rmdir` - remove directory

Lists the contents of a directory. Try running:

```
ls
```

Nothing there... let's make some folders with `mkdir`.

```
mkdir Documents
```
You can make multiple directories too

```
mkdir Pictures Projects Movies Music Temp
```

Also include spaces in the name (in two ways)

```
mkdir "2019 Documents"
mkdir 2020\ Documents
```
Have a look at what we've done with:

* `la -l` - outputs list in a single column 
* `ls -a` - lists everything (including hidden files)
* `ls -h` - lists human readable file sizes

Try:

```
ls -ahl
```

You can remove an empty directory with the `rmdir` command.

```
rmdir Temp
```

You can also remove multiple empty directories.

```
rmdir "2019 Documents" 2020\ Documents
```

### Permissions (intro)

We've been listing the contents of our working directory, but we can also list the contents of other directories on our Pi. Try:

```
ls -ahl /etc/wpa_supplicant/
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
cd /etc/wpa_supplicant
```

Run `pwd` to confirm.

#### `cp` - copy

Making a backup of a system configuration file is ALWAYS a good idea. `cp` takes two arguments, the source file to be copied and the destination file.

```
cp wpa_suplicant.conf wpa_suplicant.conf.bak 
```

Permission denied? Only the root user can write to this directory!

#### `sudo - superuser do

Use `sudo` to temporarily escalate your privileges to root or superuser.

```
sudo cp wpa_suplicant.conf wpa_suplicant.conf.bak
```

#### `nano` - text editor

Nano is a text editor.

```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

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
static ip_address=your.hotspot.subnet.100/24
```

4. Save - `control + x`, Y, enter
5. `reboot` - `sudo reboot`

Let me know when you've rebooted, and which IP address your Pi is @ on the NOSCRIPT network.