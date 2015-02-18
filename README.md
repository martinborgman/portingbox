# OpenVMS Porting Box #
This repository contains a Vagrantfile for a OpenVMS porting box using the alphavm emulator on a Ubuntu Linux box.

## Requirements ##
To use this OpenVMS Porting Box you'll need:
* a recent [VirtualBox](https://www.virtualbox.org) installation
* a recent [Vagrant](https://www.vagrantup.com) installation
* a recent [AlphaVMFree](http://emuvm.com/alphavm_free.php) kit for linux
* a porting.dd file containing a complete OpenVMS 8.4 installation ready for basic porting activities
* a terminal emulation program that supports raw IP connections like [socat](http://linux.die.net/man/1/socat) or [putty](http://www.putty.org)
* official OpenVMS licenses or OpenVMS Hobbyist licenses

## Getting Started ##
To use this Vagrantfile, do the following:
* Download the zip distribution or do a git clone to a directory of your choice
* Place the porting.dd in the same directory
* Unzip the AlphaVMFree kit and put all the files in the alphavm-free subdirectory
* Start the box with vagrant up
You are now ready to start your porting box

## Starting the Porting Box ##
Now connect to your Vagrant Box using `vagrant ssh`.
You should now be in the /home/vagrant directory on the VirtualBox VM
Start the emulator with the command alphavm_free porting.emu.

## Booting OpenVMS ##
To connect to the emulator console use a terminal emulation program that supports raw IP connections like socat on linux, socat is available on the Ubuntu linux VM so you could make a new vagrant ssh connection.
You can also use Putty on Windows.

The socat command is:

    socat -,raw,echo=0,escape=0x1c tcp:127.0.0.1:20000`

When connected to the console press enter to get a console prompt and enter:

    boot dka0

OpenVMS should boot now. If asked for date and time, please enter the correct date and time.

At the login prompt you can now login using the username system whit the password porting. You may be required to change the password.

## Entering licenses ##
You now have a bit of a chicken and egg situation. You can only login en enter the commands using the console.
You cannot start TCP/IP or do anything else that requires a license.

The Hobbyist license file you received is a DCL command procedure with lots of product licenses but you cannot get it on your porting box at this stage.

The only thing we can do is enter a few licenses manually so we can start TCP/IP and sftp the file to our box and execute it.
The licenses you'll need to enter manually are called OPENVMS-ALPHA and UCX (the old name for the TCP/IP stack).
Find these licenses in the hobbyist license file an execute the commands as shown in the file starting with:

    LICENSE RESISTER <name>
	
After entering both licenses, you can load the licenses with: 

    LICENSE LOAD

At this point you can start TCP/IP with:

    @SYS$STARTUP:TCPIP$STARTUP.COM

If all goes well should get an IP-address using DHCP and both NTP and SSH will be started. You should now be able to use sftp to transfer the Hobbyist license file over to your box.

There is a bit of a second catch at this point because the license file you received is probably an MS-DOS text file and OpenVMS doesn't like MS-DOS text file very much. Now sftp should take care of these kind of problems, but unfortunately sftp isn't particularly smart in finding the correct conversion. On Windows it largely depends on the sftp client you're using. On Linux or a Mac, simply convert the license file to a unix text format using one of the many available methods.
For those new to UNIX systems, you can use:

    tr -d '\r' < input.file > output.file 

Now you can sftp the Hobbyist license file over to your VMS box and execute it using:

    @<filename>
   
## TCP/IP setup ##
As you have seen in the previous item, TCP/IP comes preconfigured.
* The interface is configured to use DHCP
* The NTP services is enabled and started automatically.
* The SSH server en client services are enabled and started automatically.

You can enable or disable TCPIP services using:

    @SYS$MANAGER$TCPIP$SETUP

Be aware that the DHCP address is actually provided by VirtualBox and that the VirtualBox vm is using bridged networking by default.

Also note that port forwarding is configured to connect port 20000, your console port, to port 20000 on your local machine.

## Porting Software ##
To be able to port common (UNIX/linux) software to OpenVMS you'll need an environment that looks and feels like a linux, usually bash, environment. This environment is provided by GNU is Not VMS or GNV for short.
Before you start or any other of the GNV programs, enable extended parsing for your process by entering:

    SET PROCESS/PARSE_STYLE=EXTENDED

Also note that UNIX or UNIX like shells give a special meaning to the '$' sign. On OpenVMS '$' signs can be used in file an directory names and this can be confusing for some GNV tools. It is probably best to avoid file an directory names with '$' signs when porting software.

### GNV ###
GNV is installed and configured for use on the porting box.
GNV provides a Linux like environment on OpenVMS and provides a BASH shell to use this environment.
GNV provides a large set of Linux standard commands ported over to OpenVMS.
To be able to port software yourself, GNV provides GNU make and a set of wrappers for cc, gcc, c++, ar, ld, etcetera. However at this point GNV does not provide a complete set of GNU build tools yet.

### BASH ###
GNV provides a BASH shell. On this porting box you will find a more recent version of BASH however.
You can start bash from DCL by entering the `BASH` command.

### GCC ###
GNV provides a set of wrappers for OpenVMS commands like CC, CPP, LIBRARY, and LINK to mimic their GNU counterparts such as the gcc suite. They provide only a general subset of the complete functionality of the gcc suite.
Be aware there are hacks available to support certain situations. The GNV wrapper tools provide a help option to document these hacks.

## Shutting down the AlphaVMFree ##
After normal OpenVMS `SHUTDOWN` use the emulators `power` command in the console to shut the emulator down.
Please don't use `Ctrl-C` to shut down the emulator.
