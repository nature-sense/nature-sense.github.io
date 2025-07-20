---


title: Installing the System
parent: Software Installation
nav_order: 2

---

# Installing the System

Raspberry Pi OS is installed on the Compute Module's internal eMMC. The eMMC can be made to act like and the OS can be flashed directly to it using the standard raspberry tools.

## rpiboot

This process requires a command line tool to put the eMMC into the right mode. This tool must be  installed manually.  Unfortunately, on the Mac there is no installable version of this tool, and it must be built locally.  

> [!IMPORTANT]
>
> You only need to do this once. It permanently installs rpiboot on your computer, so for successive uses you can simply run it.

#### 

#### Installation on a Mac

It must be built from the command line (Terminal) and the steps are as follows:

Install `brew` if you don't have it already.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then clone the GitHub repository and build `rpiboot`

```shell
git clone --recurse-submodules --shallow-submodules --depth=1 https://github.com/raspberrypi/usbboot
cd usbboot
brew install libusb
brew install pkg-config
sudo make INSTALL_PREFIX=/usr/local install
```

#### Other Platforms

For other platforms see  [here](https://github.com/raspberrypi/usbboot).

## Flashing the Software

### Flash Mode

The first step is to put the board into Flash Mode.  There is a button next to the USB-A socket to do this.

![flash-mode](../images/flash-mode.png)The steps are as follows:

- Plug a USB-C cable into the module but leave the other end disconnected. Using a small screw-driver or similar and press and hold the flash-mode button.
- With the button still pressed plug the other end of the USB-C cable into your computer.
- Release the button. 

The Compute Module is now in "flash mode". 

DO NOT DISCONNECT THE TRAP FROM YOUR COMPUTER.

## rpiboot

Now the Compute Module is in flash mode you can run rpiboot to configure rhe eMMC as a memory stick:

```
sudo rpiboot
```

This is an example of the console output when it runs.

```shell
(base) steve@elnor ~ % sudo rpiboot
Password:
RPIBOOT: build-date 2025/07/16 pkg-version local e896b5b8

Please fit the EMMC_DISABLE / nRPIBOOT jumper before connecting the power and USB cables to the target device.
If the device fails to connect then please see https://rpltd.co/rpiboot for debugging tips.

Waiting for BCM2835/6/7/2711/2712...

Directory not specified - trying default /usr/local/share/rpiboot/mass-storage-gadget64/
Sending bootcode.bin
Successful read 4 bytes
Waiting for BCM2835/6/7/2711/2712...

Second stage boot server
File read: mcb.bin
File read: memsys00.bin
File read: memsys01.bin
File read: memsys02.bin
File read: memsys03.bin
File read: bootmain
Loading: /usr/local/share/rpiboot/mass-storage-gadget64//config.txt
File read: config.txt
Loading: /usr/local/share/rpiboot/mass-storage-gadget64//boot.img
File read: boot.img
Second stage boot server done
(base) steve@elnor ~ %
```



> [!CAUTION]
>
> The stap can be a little bit tricky and may have to repeated a few times before it is successful.

DO NOT DISCONNECT THE TRAP FROM YOUR COMPUTER.

## Raspberry Pi Imager

The OS can now be installed using the standard Raspberry Pi Imager software.

![pi-imager](../images/pi-imager.png)

- Set the 'Raspberry Pi Device' to 'Raspberry Pi 5'.
- Set the 'Operating System' to 'Raspberry Pi OS Lite (64bit)'
- Set the storage to be the 16GB eMMC from the CM5 - the only entry in the storage list.

The Settings should be configured as follows:

![settings](../images/settings.png)

- Hostname - of your choice. The trap will be accessible on wifi with this name, and the same name is used to identify it on bluetooth.
- Username - "trap"
- Password - of your choice.
- Wireless LAN - SSID and Password - to match your local network.

Save these then continue to write the OS onto thr eMMC.

When complete unplug and replug the USB-C cable to reset the trap.
