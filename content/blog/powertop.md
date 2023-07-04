---
title: "PowerTOP at boot and startup scripts"
date: 2016-09-30
slug: 'powertop-and-linux-startup-scripts'
tag: 'tutorials'
---

So, you have a laptop on which you've installed Linux, and you've configured and used PowerTOP to reduce the power consumption of said hardware. That's great! The question now becomes: "How should I implement these power-saving settings automatically every time I reboot my laptop?"
<!--more-->

So, you have a laptop on which you've installed Linux, and you've configured and used PowerTOP to reduce the power consumption of said hardware. That's great! The question now becomes "How should I implement these power-saving settings automatically every time I reboot my laptop?". You have two slightly different options, depending on which of the following strategies you used:

1. Upon visiting the *Tunables* tab, you tested each tunable item individually to ensure that optimizing its power consumption introduced no system instabilities; or
2. You said "That's too much tinkering for me, can't we do this in some easier manner?" and have auto-tuned everything from `Bad` to `Good`.

Either way, any changes you make will be lost upon reboot. In order to make it persist, you have to tell PowerTOP to run every time the machine starts. That's an easy thing to do.

### Using startup scripts the right way ###
I'll cut right to the chase: generally speaking, there's a right and a wrong way to use startup scripts. The right way is to write the script, save it in `/usr/local/bin`, and call it at boot from `/etc/rc.local`. First, it's important to do this for everything--even single-line commands, even if you think it would be easier to just write the command in this file and let it run at boot. You'll want to take this approach for the same reasons that you should avoid magic numbers in code.

Second, I strongly feel that you should store any script you write in `/usr/local/bin` rather than `/usr/bin`, even if you're the only user for that machine. It can be a dangerous gambit to play in `/usr/bin`--the system-level binaries directory--from the command line, particularly if something doesn't run as expected and a user is required to make edits in quick succession. Keeping your custom-made or -loaded binaries in `/usr/local/bin` can help keep your scripts easy to find and maintain the integrity/resilience of your system.  

I think the wrong way use startup scripts is to do the opposite of what I just mentioned. For example, it's possible to run `powertop --auto-tune` at boot just by writing that line and saving it in `rc.local`. That's an example of a single-line command that's not included in a script. For a more correct solution, see the final section.

### Creating your startup script ###
As mentioned before, this post supposes that you've successfully configured and run PowerTOP and have isolated the commands necessary for tuning each of the parameters you want to tune (and if you need an explainer, see their [user guide](https://01.org/sites/default/files/page/powertop_users_guide_201412.pdf)). Once you've completed this step...

1. From the terminal, create a startup script by typing `sudo touch /usr/local/bin/powertop.sh`. If you want, navigate to the directory and verify that the empty file was created.
2. While in this file's directory, go ahead and make the file executable (it won't be system-executable by default) by typing `sudo chmod +x powertop.sh`.
3. Once this is done, next we're going to use the terminal to generate an HTML report with the `powertop --html` command. This will generate a file named `powertop.html` in your `/home` folder.
4. Open the file with a web browser, navigate to the "Tuning" tab, and you'll find commands in the right-hand column necessary to tune their corresponding parameters in the left-hand column.
5. For each parameter that you've decided to tune, copy only the command and paste it into the file you created.

	Once you're done, it will probably look similar to this:

        #!/usr/bin/env bash
        iw dev wlan0 set power_save off
        echo 'min_power' > '/sys/class/scsi_host/host0/link_power_management_policy';
        echo '0' > '/proc/sys/kernel/nmi_watchdog';
        echo '1500' > '/proc/sys/vm/dirty_writeback_centisecs';
        echo '1' > '/sys/module/snd_hda_intel/parameters/power_save';
        echo 'auto' > '/sys/bus/usb/devices/2-6/power/control';
        echo 'auto' > '/sys/bus/usb/devices/5-1/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1f.3/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:00.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:02.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1a.7/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1b.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:02:00.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1a.1/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1f.1/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1d.1/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1d.2/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1d.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1c.4/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1a.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1c.0/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1f.2/power/control';
        echo 'auto' > '/sys/bus/pci/devices/0000:00:1d.7/power/control';
        ethtool -s eth0 wol d;

    If you aren't familiar with startup scripts, note that it always requires a shebang (and nothing else) on the first line. In simple terms, the shebang tells the system how to interpret and execute the file. In this example, we've specified bash as the shell. This is important because scripts usually include shell-specific syntax, so if you've written your script and it uses bash syntax, you'll want to give the system a directive to execute the script by using bash.

    It's also important to note that I've written `#!usr/bin/env bash` rather than `#!/bin/bash` because bash does not necessarily live in `/bin` on all systems. Because I want this tutorial to be useful for a wide variety of users who may be using different Linux distributions, I wanted to structure the line so it finds bash wherever it lives. If you're interested in finding it yourself, try `echo $SHELL` in your terminal.

6. Once all of the desired commands have been copied over and the file is saved, find and edit `/etc/rc.local`.

    It should already have something like this in the file:

        #!/bin/sh -e
	    #
	    # rc.local
	    #
	    # This script is executed at the end of each multiuser runlevel.
	    # Make sure that the script will "exit 0" on success or any other
	    # value on error.
	    #
	    # In order to enable or disable this script just change the execution
	    # bits.
	    #
	    # By default this script does nothing.

    Directly below the lines that have been commented out, add the script you just created to `rc.local`. It should now look like this:

        #!/bin/sh -e
	    #
	    # rc.local
	    #
	    # This script is executed at the end of each multiuser runlevel.
	    # Make sure that the script will "exit 0" on success or any other
	    # value on error.
	    #
	    # In order to enable or disable this script just change the execution
	    # bits.
	    #
	    # By default this script does nothing.

	    /usr/local/bin/powertop.sh

	    exit 0

7. Ensure that the script is executable, because it's not by default. Once you've navigated to `/usr/local/bin`, change its permissions like so: `chmod +x powertop.sh`.

That's it! Save the file, close everything down, and restart. Verify that the script was loaded at startup by running PowerTOP again and seeing if the parameters you selected for tuning are in fact set to `Good`.

### So what if you wanted to take the easy way out? ###

At the top I said that one could take the easy approach to the problem and, rather than manually specifying which parameters you'd like to tune, you just want to allow PowerTOP to tune the entire machine--consequences or no. If that's the case, you can ask PowerTOP to auto-tune the machine at boot, and the procedure is fairly straightforward.

1. Follow steps 1 and 2 from the directions set in the section above.
2. Rather than manually copying and pasting the tuning commands into the file, instead replace it with the auto-tune command.

    It should look like this:

        #!/usr/bin/env bash
        powertop --auto-tune

    That's it! All this script does is call the auto-tune function included natively with PowerTOP, rather than specifying a series of individual file changes to be made as we did in the first script we wrote.

3. Complete everything in step 6 from the section above, editing `rc.local` and verifying that changes were made by running PowerTOP again. It may seem a little unnecessarily convoluted to take this path, and it may seem trivial enough that you could easily just call the auto-tune function from within `rc.local` on its own, but if I'm going to do any script writing I like to try to follow best practices. Ultimately, it's up to each individual user to decide what's best for his or her system.

And that's it! Hopefully this has been helpful and you've learned a little along the way. If you have any questions or think that something is erroneous, please send me an email through the link at the top of the page.
