# Thinkpad T430 - Hackintosh - OpenCore
[![T430](https://img.shields.io/badge/ThinkPad-T430-blueviolet.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_T430.pdf)
[![OC](https://img.shields.io/badge/OpenCore-0.9.2-informational.svg)](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.9.2)
[![10.13](https://img.shields.io/badge/macOS-10.13-yellowgreen.svg)]()
[![10.14](https://img.shields.io/badge/macOS-10.14-blue.svg)]()
[![10.15](https://img.shields.io/badge/macOS-10.15-9cf.svg)]()
[![11](https://img.shields.io/badge/macOS-11-red.svg)]()
[![12](https://img.shields.io/badge/macOS-12-blueviolet.svg)]()
[![13](https://img.shields.io/badge/macOS-13-yellow.svg)]()
[![download](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/jozews321/T430-Hackintosh-Opencore/releases/latest)
<img align="left" src="/resources/T430-new.png" alt="Lenovo Thinkpad T430" width="300">
<img align="right" src="/resources/homepage.png" alt="Opencore" width="200">
<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
## SUMMARY 
This guide will provide a EFI folder configured to install macOS 10.13 to 12 on the Thinkpad T430 using OpenCore Bootloader with every device working with some exceptions depending on your particular T430 model (see below)

|:warning: This guide assumes prior knowledge on how to do basic Hackintosh stuff |
|:--------------------------------------------------------------------|
If you are new to this i suggest you to go to [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) first
There will be references to the linked guide throught this proccess for those that are new to this

## REQUIREMENTS
- Lenovo Thinkpad T430 (T430s is similar enough to work with this config, but i can't verify)
- Intel Core i3 3110M or better (Celeron and Pentium CPUs are not supported)
- Integrated Intel HD4000 graphics (NVIDIA discrete GPUs are not supported)
- At least 4GB RAM 
- Stock Intel Centrino WiFI card (optional)
- Internal Broadcom BCM20702 Bluetooth 4.2 card (optional)
- Integrated Fingerprint reader and WWAN card are not supported
- Dual Booting is discouraged in this guide as there is many boot scenarios to cover properly here
- SSD (HDDs suck)
- USB drive (at least 16gb for full install or 2gb for internet recovery)

## ABOUT
The provided ACPI tables in this OC EFI folder are made using a DSDT-less method via hotpatching to increase compatibility with different BIOS versions.

## HARDWARE TESTED
Do a pull request to add more Hardware configs to this list
<details>
<summary><strong>My T430</strong></summary>

### ThinkPad T430 Specs 
| Component           | Details                                       |
| ------------------: | :-------------------------------------------- |
| Model               | Lenovo ThinkPad T430                          |
| BIOS Version        | 2.77, unlocked with 1vyRain                   |
| Processor           | Intel Core i7 3610QM                          |
| Memory              | 16GB DDR3 1600MHz in Dual-Channel             |
| SSD                 | Intel 520 Series SSD 180GB                    |
| Graphics            | Intel HD Graphics 4000                        |
| Display             | 15.6" 1600x900                                |
| Audio               | Realtek ALC269VC                              |
| Ethernet            | Intel 82579LM Gigabit Network                 |
| WIFI                | Intel Dual Band Wireless-AC 7260              |
| Bluetooth           | Integrated Broadcom BCM20702 Bluetooth 4.2    |
  
</details>


## INSTALL GUIDE
<details>
<summary><strong>Getting the EFI ready</strong></summary>	
Download the latest release [T430 EFI](https://github.com/jozews321/T430-Hackintosh-Opencore/releases/latest)
	
- If you are going to install macOS 11 to 13 you must use the install only config.plist, remove the "[InstallOnly]" so it reads "config.plist" (Don't skip this step)
- If you are installing 10.13 to 10.15 it's not necessary to use the install only config, so use the post install one, remove the the "[PostInstall]" so it reads "config.plist"
	
</details>
<details>
<summary><strong>BIOS Settings</strong></summary>

### BIOS Settings
Latest BIOS Version: `2.77` stock or ivyrain

**CONFIG TAB**

* USB UEFI BIOS Support: `Enabled`
* USB 3.0 Mode: `Enabled`
* Display > Boot Display Device: `ThinkPad LCD`
* SATA > SATA Controller Mode: `XHCI`
* CPU > Core Multi-Processing: `Enabled`
* CPU > Intel (R) Hyper-Threading: `Enabled`

**SECURITY TAB**

* Security Chip: `Disabled`
* UEFI BIOS Update Options > Flash BIOS Updating by End-Users: `Enabled`
* UEFI BIOS Update Options > Secure Rollback Prevention: `Enabled`
* Memory Protection: `Enabled`
* Virtualization > Intel (R) Virtualization Technology: `Enabled` 
* I/O Port Access (`Disable` the following:):
	* Wireless WAN
	* ExpressCard Slot
	* eSATA Port
	* Fingerprint Reader
	* Antitheft and Computrace
	* Secure Boot: `Disabled`

**STARTUP TAB**

* Boot (HDD/SSD as first device)
* UEFI/Legacy Boot: `UEFI only`
* CSM Support: `Disabled`
* Boot Mode: `Quick`

</details>

<details>
<summary><strong>USB Installation media</strong></summary>

### Creating the USB installer 
In this step you will create a macOS installation media.
Regardless of the OS you are using to create the installer you will need some tools [Python](https://www.python.org/downloads/), [Propertree](https://github.com/corpnewt/ProperTree)
<br /> <br /> 
Now go to [OpenCore Guide - Creating the USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) where you can find the instructions step by step to create the installation media with your respective OS 
</details>

<details>
<summary><strong>Adding the EFI folder to the USB</strong></summary>
<br /> 
Now you will need to copy the EFI folder to the root of your USB Installer in order to boot from it 
<br /> <br /> 
	
You can consult [OpenCore Guide - Creating the USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) to get some instructions on how to do this with your OS
 
</details> 

<details>
<summary><strong>Installing macOS</strong></summary>
<br /> 
Boot from the USB by pressing F12 on the Thinkpad BIOS and choose your USB

- You will see the OpenCore Boot Picker and choose to boot from your installation media

- After that select Disk Utility and format your HDD/SSD in APFS

- If running the internet installer connect an ethernet cable right now or connect WIFI or use an Android phone to tether via USB	
	
- Install as normal
	
You can consult [OpenCore Guide - Installation Process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html) to get some instructions if you need them.
</details> 

## POST INSTALL
Follow the next steps carefully to get a fully working system 

<details>
<summary><strong>Switching to Post Install config and generating SMBIOS serial</strong></summary>
<br />
After macOS is installed you will need to switch to using the post install config.plist
	
- Go to EFI/OC and delete the install only config.plist and remove the "[PostInstall]" so it reads only "config.plist".
	
Now it's time to generate the Serial, MLB, UUID and ROM to the config.plist (you will need to have ProperTree installed)

- Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS/)
  <br /> <br /> 
- Open config.plist with ProperTree in the EFI folder
  <br /> <br /> 
- Open GenSMBIOS
  <br /> <br /> 
- Choose 1 to install MacSerial
  <br /> <br /> 
- Choose 3 to generate some new serials
  <br /> <br /> 
- Write MacBookPro10,1
  <br /> <br /> 
- You will get something like this
  <br /> <br />  
<img src="/resources/gensmbios.png" width="600">
  <br /> <br /> 
  
- If you care about iServices you will need to try the generated serial in [Apple Coverage](https://checkcoverage.apple.com)
  and try to get this message (use a VPN or TOR to get around the validation limits) 
<img src="/resources/notvalidated.png" width="600">
   <br /> <br /> 
  
- Add the generated serials in the config.plist at /PlatformInfo/Generic
  (Serial to SystemSerialNumber, Board Serial to MLB, SmUUID to SystemUUID, AppleRom to ROM)
<img src="/resources/configsmbios.png" width="600">
   <br /> <br /> 

- Save and continue
</details> 

<details>
<summary><strong>macOS 12 and 13 - HD 4000 system patch</strong></summary>
<br /> 
Apple dropped the HD 4000 iGPU with macOS 12. If you dont install this you won't have any kind of graphics acceleration and your macOS 12 and 13 experience will be completely miserable

- Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)

- Run OLCP
	
- Choose Post Install Root Patch and follow instructions
	
- Reboot
	
If everything went right, now you would be able to control the brightness and enjoy fully Metal accelerated UI
</details> 


<details>
<summary><strong>Boot without USB</strong></summary>
<br /> 

- Download [MountEFI](https://github.com/corpnewt/MountEFI)

- Choose your macOS drive and it should be mounted in Finder 
	
- Copy your EFI folder to the root of the EFI partition on your macOS drive
	
- Reboot and disconnect your USB drive
	
- Boot from disk
	
</details> 

<details>
<summary><strong>Optimizing CPU power management</strong></summary>
<br /> 
Follow this guide(Don't skip or your cpu will be locked at minimum or base clock and everything will be very slow):
https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management

	
</details> 

<details>
<summary><strong>Disable verbose boot</strong></summary>
<br /> 
If you managed to boot without any issues you can disable the verbose boot to get a clean boot experience 

- Open the config.plist

- Go to NVRAM/Add/7C436110-AB2A-4BBB-A880-FE41995C9F82
	
- Find boot-args and delete  `-v`
	
- Reboot
	
</details> 

## Credits

Thanks to:

* [Apple](https://www.apple.com) (macOS)
* [Acidanthera](https://github.com/acidanthera) (OpenCore, VirtualSMC, Lilu, WhateverGreen and a lot more)
* [Dortania](https://dortania.github.io) (Opencore Install Guide, Opencore Legacy Patcher)
* [OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm) (Airportitlwm)
* [5T33Z0](https://github.com/5T33Z0/Lenovo-T530-Hackinosh-OpenCore) (T530 ACPI fixes)
* [zhen-zen](https://github.com/zhen-zen/YogaSMC) (YogaSMC)











 











