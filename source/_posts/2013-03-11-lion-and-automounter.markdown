---
layout: post
title: "Lion &amp; Automounter"
date: 2013-03-11 11:05
comments: false
categories:
---
Mounting a remote filesystem via the network is something which Unix has been doing since,
well, the dawn of Unix. So, naturally, when trying to make use of large amounts of network
storage on a NAS from a Mac OS X based machine, one would assume that mounting the network
storage and having the mountpoint be maintained automatically should be a simple Unix
configuration. And there you'd be partly correct...

Darwin does make use of the `automount` system to configure and maintain mountpoints, and
one can setup entries in the `/etc/fstab` to mount remote filesystems using various
protocols (NFS, SMB, AFP, etc.), and various posts about doing this have been around for
[a long time](http://web.archive.org/web/20090831103644/http://blogs.sun.com/lowbit/entry/easy_afp_autmount_on_os)
. However, Mac OSX Lion [seems to have broken this](https://discussions.apple.com/thread/3221944)
in such a way that the mount points are inaccessible (incorrect permissions) to all but
the `root` user.

Over the weekend I struggled with this for too long, and ultimately found a solution,
albeit a bit of a hack. Roll up your sleeves, get a new cup of tea, here we go...

###/etc/fstab
As root, edit your `/etc/fstab` and add the mount point you want.
Note: `/etc/fstab` did not exist by default. No need to worry, just create it. The _magic_
syntax is:

	<servername>:/<path> <mount_point> url auto,url==afp://<username>:<password>@<servername>/<path> 0 0

Example:

	nas.grokers.net:/media /Network/media url auto,url==afp://levi:foopass@nas.grokers.net/media 0 0

That sets up a mount point at `/Network/media` which points to the AFP share `nas.grokers.net/media`

If you do this, then tell `automount` to reload with `automount -cv` you'll be able to cd into
`/Network/media` and see the remote filesystem, but only as root (in Lion, at least).

As an aside, it pains me to embed a password in the filesystem like this. A long time ago,
I spent a lot of effort trying to figure out a way to dynamically load the password from
the Keychain instead of embedding it here. I researched how to use executable automount
configurations so I could fetch the password using `/usr/bin/security` but ultimately
could never get it to work, so I gave up. If you've a way to do this, please [let me know](mailto:levigroker@gmail.com).

###root-only permissions work-around
To avoid the root-only permissions on the mount point, it would appear a [solution](http://forums.plexapp.com/index.php/topic/14201-howto-automount-afpsmb-shares-using-autofs/?p=202429)
is to unmount the mountpoint after `automount` loads the configuration in `/etc/fstab`.

I tried many different approaches to unmount the mountpoint using a `launchd` `/Library/LaunchAgent`
and even a user LaunchAgent, but through many attempts none of them seemed to take place
_after_ `automount` had loaded the config, and thus the mountpoint was still root-only
accessible.

So, an AppleScript application as a Login Item seems to be the only way, but we don't have
to embed our password (yet again) in the applescript. Instead, I created a shell script to
do the deed:

	#!/bin/bash
	diskutil unmount /Network/media > /dev/null 2>&1
	exit 0

(I saved it at `/Users/levi/Library/Automation/nas_mount_helper.sh` but it doesn't matter where you put it)

Then add a line to the `sudoers` file so the script will run as `root` without the need
for a password:

`$ sudo visudo`

	levi ALL = NOPASSWD: /Users/levi/Library/Automation/nas_mount_helper.sh

Now, to run this script as a Login Item, I created an AppleScript application using the
AppleScript editor app. The contents of the applescript is pretty basic:

	do shell script "sudo /Users/levi/Library/Automation/nas_mount_helper.sh"

Save the script as an application, and use the "Users & Groups" System Preferences to add
it as a Login Item

Almost there...

Unfortunately, this didn't quite do it. Upon login the mount couldn't be navigated to via
the Finder because of some error about the original not being found, so...

Another shell script was needed to force the mount.

	#!/bin/bash
	cd /Network/media
	ls -la
	exit 0

(I saved it at `/Users/levi/Library/Automation/nas_mount_helper2.sh` but it doesn't matter where you put it)

This one we want to run as ourself, so no `sudoers` entry is needed.

Add an additional line to the applescript to run this after we unmount the share:

	do shell script "sudo /Users/levi/Library/Automation/nas_mount_helper.sh"
	do shell script "/Users/levi/Library/Automation/nas_mount_helper2.sh"

### Extra Credit

Finally, to prevent the AppleScript application from being noticeable (showing up in the dock, etc.),
add the `LSUIElement` key to the `Info.plist` of the generated AppleScript application,
by contextually clicking on the app from the Finder, choosing "Show Package Contents",
browse to `Contents/Info.plist`, and then edit the Info.plist to add the element:

	<plist version="1.0">
	  <dict>
	    ...
		<key>LSUIElement</key>
		<string>1</string>
		...
	  </dict>
	</plist>

Not all that elegant, unfortunately, but a workaround at least.

### Thanks

Thanks to [@signed8bit](https://twitter.com/signed8bit) for the moral support, and the wayback-machine.  
Thanks to [Soli](http://forums.plexapp.com/index.php/user/22159-soli/) for the unmount workaround idea.

### Comments

[Max Yarchak](http://MaxProWeb.com)
> I face the same issue. I've build a script which does reconnect to wifi (network) and it solves the issue with permissions for me.  
> [https://github.com/MaxProWeb/MacAutomountFix](https://github.com/MaxProWeb/MacAutomountFix)
