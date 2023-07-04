---
title: "Installing Pi-hole on a BeagleBoard to block ads"
date: 2017-01-04
slug: 'pi-hole-on-beagleboard'
tag: 'tutorials'
---

Instructions for installing Pi-hole to systematically block advertisements across your network are easy to find, but it gets a little more tricky when using different hardware. Here are some tips to get up and running with a BeagleBoard.
<!--more-->

### A note to readers ###
I've updated this post several times after getting comments from readers and helping them troubleshoot their installations. As you read through, note the changes at the bottom of the page. If you experience other issues, write me, we'll figure it out, and I'll make the update to this page. 

### Original post ###
I've had a BeagleBoard sitting around unutilized since last winter, and I wanted to find a fun little project for it. I ran across [Pi-hole](https://pi-hole.net/), a program that can be installed on a Raspberry Pi and used to block ads at the network level (as opposed to doing so from the browsers of each individual machine on your home internet network). It's a simple process to get up and running:

* Connect your hardware to your router via ethernet cable;
* Log in to your hardware and install Pi-hole;
* Log in to your router and manually set its DNS IP address to your hardware's IP address
* Reboot both devices and see ads blocked on all of your computers and devices!

I had a BeagleBoard sitting around rather than an RPi, so I decided to use that instead (and for good reason: Raspbian, the recommended software on which to install Pi-hole, is just Debian optimized for the RPi, so I expected good interoperability). I can tell you that it does work very well, but there were a few snags that made it a little less than straightforward. This tutorial will focus on solutions to those problems.

### My deployment ###
Just so you're clear, here's what I'm using:

* element14 BeagleBone Black (BBB) rev. C, connected via ethernet to my router;
* Asus router;
* Debian 8.6 ("Jessie") [IoT image](https://beagleboard.org/latest-images), selected because it's a minimal installation that supports the BBB's onboard connectivity ports but eschews GUI elements (window managers, desktop environments, etc). Installed is only ~1.6gb.

I flashed the image to the onboard eMMC using [these instructions](http://derekmolloy.ie/write-a-new-image-to-the-beaglebone-black/), rather than running it full-time from the SD card itself. During flashing, remember that the LEDs will light up in a pattern (1-2-3-4-3-2-1-2-3-4....) and then go all solid when complete; if you're having trouble flashing, navigate to `/boot/uEnv.txt` on the SD card and _uncomment_ the following line:

    cmdline=init=/opt/scripts/tools/eMMC/init-eMMC-flasher-v3.sh

You will also want to modify the following lines in that same file, changing ```enable``` to ```disable```:

    cmdline=coherent_pool=1M quiet net.ifnames=0 cape_universal=disable

As well as changing the following ```1``` to ```0```:

    enable_uboot_cape_universal=1

Details on these variable changes can be found at the bottom of this post under the 3/21/2017 update.

Once flashing is complete, shut down the BBB, remove the SD card, reboot it, and update the software. At this point you would normally be ready to install Pi-hole, but for this tutorial I'm going to work backwards. The two tasks below are things I discovered I had to do only after the software was installed, so rather than walk you through that way (and risk the need to reinstall Pi-hole if something doesn't work out), we'll take care of these two things first. Note that you could also use these tasks to troubleshoot an existing BBB installation that isn't working.

### First task: Uninstalling ConnMan to free port 53 ###
The first thing I encountered after installing Pi-hole was that I couldn't actually visit any websites after switching the router DNS settings to point to the BBB. It would work fine if I flipped my router back to the OpenDNS IPs. I could ping Google from my desktop and from the BBB over ssh, which told me that traffic could get out if directed to specific IPs, but the inability to put a URL in a browser and get a website seemed to imply that DNS wasn't able to actually resolve any IPs.

Since Pi-hole prefers [dnsmasq](https://wiki.archlinux.org/index.php/dnsmasq) as the resolver program, I decided to look at its config file (found at `/etc/dnsmasq.conf`) for obvious problems and check its syntax (`dnsmasq --test`). Everything turned out fine and syntax was OK.

Next, I checked its status (`systemctl status dnsmasq.service`) and got a __failure.__ However, it wasn't very verbose and only really gave me an "exit-code: 2" error. I then resorted to `sudo dnsmasq` to check the output, and got in return: `dnsmasq: failed to create listening socket for port 53: Address already in use!` This indicated that something was already bound to and listening on port 53, which was preventing dnsmasq from doing so and thereby disallowing DNS resolution.

To determine what it was, I asked for the list of programs listening to that port. Running `lsof +M -nPi :53` gave me:

    COMMAND   PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    connmand 3825    root   4u   IPv4 165918      0t0  UDP * :53
    connmand 3825    root   5u   IPv4 165919      0t0  TCP * :53 (LISTEN)
    connmand 3825    root   6u   IPv6 165920      0t0  UDP * :53
    connmand 3825    root   7u   IPv6 165921      0t0  TCP * :53 (LISTEN)


If dnsmasq was configured correctly, you would see that under "COMMAND" rather than connmand (or connman-daemon). [Connman](https://wiki.archlinux.org/index.php/Connman) was present here because that's the default network manager bundled with the IoT image I was using (see [this thread](https://groups.google.com/forum/#!topic/beagleboard/7pex-V9vYWw) for details about why it's included, aside from the fact that it's designed for embedded devices).

This portion can be a bit tricky, because connman is likely to be this system's default (and only) network manager; if you uninstall it without another management utility in place, you'll be stuck without a way to ssh back into the system. The solution is to install network-manager (`apt-get install network-manager`), which will recognize the dependencies __and__ uninstall connman for you. Once you do this, give it a few minutes and resist the urge to restart the BBB; network-manager will come back up without starting a new ssh session, even though it hangs in the middle of the process. Try to be patient!

As an aside, if you would rather keep connman around and just configure the network interface utilities more intelligently, it's possible to get connman and network-manager to play nicely together. See [this discussion](https://groups.google.com/forum/#!searchin/beagleboard/neutrinos4all%7Csort:relevance/beagleboard/-Ev4jf03EQ4/EjyANDGWFgAJ) for details and the associated commands.

Once this was done, I checked dnsmasq again (`systemctl status dnsmasq.service`) and found it to be running. Success! If you find that things still aren't working and you've tried to restart networking (`/etc/init.d/networking restart`), note that rebooting the BBB should take care of it.

### Second task: Uninstalling node.js to free port 80 ###

The second thing I encountered, after getting the DNS resolution to work and checking from the command line that Pi-hole was working, was an inability to access the web interface to manage. Typing `http://type.your.pihole.ip/admin/` into your browser's URL bar should take you to a nifty little web interface, and from here you can see stats about ads blocked, DNS queries forwarded, etc. It's not necessary for a fully functional Pi-hole, but it's fun and I couldn't access it.

The browser gave some HTML that said "Couldn't GET /admin/," so because the GET request was failing I thought that the HTTP content needed by the web interface couldn't be retrieved over port 80. I used the same `lsof` trick used above, but this time substituted port 80 for 53:

     COMMAND   PID     USER    FD  TYPE   DEVICE SIZE/OFF  NODE NAME
     nodejs   1214 www-data    5u  IPv4   17813       0t0  TCP * :80 (LISTEN)
     nodejs   1214 www-data    6u  IPv6   17814       0t0  TCP * :80 (LISTEN)

So, here the same thing was happening, but this time [node.js](https://wiki.archlinux.org/index.php/Node.js_) was sitting on port 80. This was also a feature bundled with the BeagleBone software, allowing users to use its included node.js [BoneScript](http://beagleboard.org/support/bonescript) library to interface with the BBB via browser. The problem, of course, is that it uses port 80 to deliver that experience to your browser. Pi-hole needs that port in order to deliver the web interface with [lighttpd](https://wiki.archlinux.org/index.php/lighttpd), so the two are at odds (and since the node.js utilities are probably called pretty early during BBB startup to support those functions, it probably beats the lighttpd initialization calls that were installed post-hoc on top of the system software by Pi-hole).

Uninstalling node.js also removed bonescript, cloud9, and some other things, freeing up yet more space on the BBB. In order to work backwards to node.js and avoid potential "unmet dependency" errors, execute the following commands: 

    sudo apt remove --purge bonescript
    sudo apt remove --purge c9-core-installer
    sudo apt remove --purge nodejs

Once complete, I restarted the service (`/etc/init.d/lighttpd restart`) and reloaded the web interface. It worked! Problem solved.

### Third task: Installing Pi-hole ###
Now that these problems are out of the way, you can proceed with the actual Pi-hole installation. This was the easiest thing to do, as following [these](https://www.youtube.com/watch?v=TzFLJqUeirA) [instructions](https://www.youtube.com/watch?v=y7w2W2HXt3s) worked without issue following default settings. If problems arise, don't forget that you can debug (`pihole -d`), reconfigure or reinstall (`pihole -r`), and check that the Gravity script is actually pulling updated blocking lists (`pihole -g`).

Hopefully this tutorial was helpful. If you have any questions or comments, please send me an email!

<br>

### Changelog ###

#### Update 3/21/2017 ####
After some experience with this setup, I noticed (`df -h`) that my ROM was being filled at an alarming rate--over the course of a 24h period, I had lost nearly a gigabyte of the BBB's remaining memory. Almost all of it was due to an exploding kern.log file, to which hundreds of entries a minute were being written with `universal cape not found` errors. It seemed that the device was looking for attached capes and, finding none, incessantly wrote these errors to kern.log.

In order to rectify this problem, I found setting those variable changes in the `uEnv.txt` file (the procedure for which is mentioned above) eliminated the problem, as these disable the BeagleBone's Universal Cape polling capability and all the errors it writes to the log file when it fails. If you were also interested in examining your device, I would recommend navigating to `/var/log` and executing this command: `du -hsx * | sort -rh | head -10`. It'll give you the top ten largest files in your present working directory, sorted by size.

<br>

#### Update 12/31/18 ####
I added some detail to the process for uninstalling node.js, namely working backwards from `bonescript` to `node.js` to avoid getting "unmet dependencies" errors. I also added detail about the `uEnv.txt` line. Thanks, Harold!
