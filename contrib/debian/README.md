Debian
======

This directory contains files used to package yotokend/yotoken-qt
for Debian-based Linux systems. If you compile yotokend/yotoken-qt yourself, there are some useful files here.

## yotoken: URI support ##

yotoken-qt.desktop (Gnome / Open Desktop)

To install:

	sudo desktop-file-install yotoken-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your yotoken-qt binary to `/usr/bin`
and the `../../share/pixmaps/yotoken128.png` to `/usr/share/pixmaps`

yotoken-qt.protocol (KDE)