at-low46's build of dwm
========================

dwm - dynamic window manager
----------------------------
[dwm](https://dwm.suckless.org/) is an extremely fast, small, and dynamic window manager for X.


Requirements
------------
In order to build dwm you need the Xlib header files.


Patches
-------
The website has a [list of community patches](https://dwm.suckless.org/patches/).

To manually apply a patch, run:

	patch -p1 < path/to/patch.diff

To revert it, add the -R option:

	patch -p1 -R < path/to/patch.diff

Remember that patches only modify config.def.h, so you will need to manually replace/delete/update config.h before compiling.

Sometimes, after multiple patches, patching may fail and will require manual intervention. In that case, manually apply the unsuccessful modifications found in a newly generated file ending in a .rej extension.


Installation
------------
Edit [config.mk](config.mk) to match your local setup (dwm is installed into
the /usr/local namespace by default).

Afterwards enter the following command to build and install dwm (if
necessary as root):

	make clean install


Running dwm
-----------
Add the following line to your .xinitrc to start dwm using startx:

	exec dwm

In order to connect dwm to a specific display, make sure that
the DISPLAY environment variable is set correctly, e.g.:

	DISPLAY=foo.bar:1 exec dwm

(This will start dwm on display :1 of the host foo.bar.)

In order to display status info in the bar, you can do something
like this in your .xinitrc:

```sh
while xsetroot -name "`date` `uptime | sed 's/.*,//'`"
do
	sleep 1
done &
exec dwm
```

Configuration
-------------
The configuration of dwm is done by creating a custom config.h
and (re)compiling the source code.

FAQ
---
> What are the bindings?

This is suckless, the source code is the documentation. Check out [config.h](config.h).

> Wait, if I change the configuration by editing the source code and recompiling, does that mean I need to close all my applications and quit the graphical environment to reload it and make the changes take effect?

Yes, but actually no. You can start dwm in a _while_ loop to reload it while not destroying other X windows.

For example, if you use the following code (taken from the [Arch Wiki](https://wiki.archlinux.org/index.php/Dwm)) instead of just using `exec dwm` in your .xinitrc, quitting dwm will reload it if the executable has been rebuilt.

```sh
# relaunch DWM if the binary changes, otherwise bail
csum=""
new_csum=$(sha1sum $(which dwm))
while true
do
	if [ "$csum" != "$new_csum" ]
	then
		csum=$new_csum
		dwm
	else
		exit 0
	fi
	new_csum=$(sha1sum $(which dwm))
	sleep 0.5
done
```

License
-------
In jurisdictions that recognize copyright laws, the following licenses apply:

[dwm](https://git.suckless.org/dwm) is available under the [MIT/X Consortium License](LICENSES/MIT).

Everything made by me is under [The Unlicense](LICENSES/UNLICENSE).
