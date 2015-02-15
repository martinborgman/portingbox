# OpenVMS Porting Box #
This repository contains a Vagrantfile for a OpenVMS porting box using the alphavm emulator on a Ubuntu Linux box.

## Requirements ##
To use this OpenVMS Porting Box you'll need:
* a recent VirtualBox installation
* a recent Vagrant installation
* a recent AlphaVMFree kit for linux
* a porting.dd file containing a complete OpenVMS 8.4 installation ready for besic porting activities.

## Getting Started ##
To use this Vagrantfile, do the following:
* Download the zip distribution or do a git clone to a directory of your choice
* Place the porting.dd in the same directory
* Unzip the AlphaVMFree kit and put all the files in the alphavm-free subdirectory  