# Dell Inspiron 15R 5537 macOS 10.14.3 Mojave Resources

## Hardware


| Component         | Name                             |                                                                                   |
| ----------------- | -------------------------------- | --------------------------------------------------------------------------------- |
| Processor         | Intel i7-4500U                   |                                                                                   |
| Graphics          | Intel HD Graphics 4400           |                                                                                   |
| Memory            | 8192 MB                          |
| WiFi              | Broadcom BCM94322HM8L 802.11abgn |                                                                                   |
| Audio             | Realtek ALC3223                  |                                                                                   |
| Ethernet          | Realtek RTL8106E                 | [driver](https://www.insanelymac.com/forum/files/file/259-realtekrtl8100-binary/) |
| Disk              | Samsung SSD 840 250GB            |                                                                                   |
| Optical           | HL-DT-ST DVD RW GU70N            |                                                                                   |
| Keyboard/Trackpad | PS/2                             | [driver](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller)                  |


## Other
| Name          | Description                                                                |                                                             |
| ------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------- |
| FakeSMC       | System Management Controller, required to boot                             | [download](https://github.com/RehabMan/OS-X-FakeSMC-kozlek) |
| Lilu          | Arbitrary kext and process patching on macOS, required for certain drivers | [download](https://github.com/acidanthera/Lilu)             |
| WhateverGreen | patches for graphics, requires Lilu                                        | [download](https://github.com/acidanthera/WhateverGreen)    |
| AppleALC      | Native macOS HD audio, requires Lilu                                       | [download](https://github.com/acidanthera/AppleALC)         |

## Tools

| Tool                | Description                 |                                                                                                         |
| ------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------- |
| Clover              | EFI Bootloader              | [download](https://sourceforge.net/projects/cloverefiboot/), [wiki](https://clover-wiki.zetam.org/home) |
| Clover Configurator | UI for Clover configuration | [download](https://mackie100projects.altervista.org/download/ccg/)                                      |
| HWMonitor           | UI for hardware sensors     | [download](https://github.com/RehabMan/OS-X-FakeSMC-kozlek)                                             |


## Guide

### Requirements
- 8GB+ USB drive (preferably with an activity light)
- existing macOS computer with the App Store, connected to the Internet
- Replaced original WiFi card with Broadcom BCM94322HM8L or similar

### 1. Configure BIOS settings 
Press `F2` to load BIOS settings. Change the following settings:

- Disk: AHCI
- UEFI Boot: Enabled
- Secure Boot: Disabled

### 2. Prepare bootable USB installer

1. On an existing Mac, use Disk Utility to Erase your 8GB+ USB drive with the following options:
    - Name: INSTALLER
    - Format: Mac OS Extended (Journaled)
    - Scheme: GUID Partition Map

2. Download macOS 10.14 Mojave from the App Store and copy it to your USB drive using the following command:
    
       sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/INSTALLER

3. Install Clover to the USB drive
    
    - Select "Change Install Location..." and choose the USB drive
    - Select "Customize" and select the following **additional** options:
        - Clover for UEFI booting only
        - Install Clover in the ESP
        - OsxAptioFix3Drv-64
    - Copy drivers into the `EFI/CLOVER/kexts/Other` directory
        - FakeSMC.kext
        - VoodooPS2Controller.kext
        - RealtekRTL8100.kext
        

### 3. Install macOS
Press `F12` to select boot disk, choose your USB drive. In Clover select the USB drive.

1. Use Disk Utility to format your disk. This will erase all your data! 
2. Click on the "View" button and select "Show All Devices"
3. Use the following options:
    - Name: macos
    - Format: APFS
    - Scheme: GUID Partition Map

Install following the prompts. Every time the computer reboots, press `F12` and select your USB drive. This time in Clover select your internal disk drive.

### 4. Install bootloader

- Select "Change Install Location..." and choose the internal drive
- Select "Customize" and select the following **additional** options:
   - Clover for UEFI booting only
   - Install Clover in the ESP
   - OsxAptioFix3Drv-64


### 5. Install drivers

- Copy drivers into the `macos/Library/Extensions` directory
   - FakeSMC.kext
   - VoodooPS2Controller.kext
   - RealtekRTL8100.kext


sudo kextcache -i /