# OpenVMS Porting Box #
This repository contains a Vagrantfile for a OpenVMS porting box using the alphavm emulator on a Ubuntu Linux box.

## Requirements ##
To use this OpenVMS Porting Box you'll need:
* a recent VirtualBox installation
* a recent Vagrant installation
* a recent AlphaVMFree kit for linux
* a porting.dd file containing a complete OpenVMS 8.4 installation ready for besic porting activities
* a terminal emulation program that supports raw IP connections
* official OpenVMS licenses or OpenVMS Hobbyist licenses

## Getting Started ##
To use this Vagrantfile, do the following:
* Download the zip distribution or do a git clone to a directory of your choice
* Place the porting.dd in the same directory
* Unzip the AlphaVMFree kit and put all the files in the alphavm-free subdirectory
* Start the box with vagrant up
You are now ready to start your porting box

## Starting the Porting Box ##
Now connect to your Vagrant Box using vagrant ssh.
You sould now be in the /home/vagrant directory on the VirtualBox VM
Start the emulator with the command alphavm_free porting.emu.

## Booting OpenVMS ##
To connect to the emulator console use a terminal emulation program that supports raw IP connections like socat on linux, socat is available on the Ubuntu linux VM so you could make a new vagrant ssh connection.
You can also use Putty on Windows.
The socat command is socat -,raw,echo=0,escape=0x1c tcp:127.0.0.1:20000
When cennected to the console press enter to get a console prompt and enter boot dka0 at the prompt.
OpenVMS should boot now. If asked for date and time, please enter the correct date and time.

## Updating licenses ##

## TCP/IP setup ##

### Interface Setup ###

### SSH ###

### NTP ###

### Enable/Disable additional services ###

## Porting Software ##

### GNV ###

### BASH ###

### GCC ###

## Shutting down the AlphaVMFree ##
After normal OpenVMS shutdown use the emulators power command in the console to shut the emulator down.