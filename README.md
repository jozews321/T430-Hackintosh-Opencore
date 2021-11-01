# Thinkpad T430 - Hackintosh - OpenCore
[![T430](https://img.shields.io/badge/ThinkPad-T430-blueviolet.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_T430.pdf)
[![OC](https://img.shields.io/badge/OpenCore-0.7.4-informational.svg)](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.7.4)
[![10.13](https://img.shields.io/badge/macOS-10.13-yellow.svg)]()
[![10.14](https://img.shields.io/badge/macOS-10.14-blue.svg)]()
[![10.15](https://img.shields.io/badge/macOS-10.15-9cf.svg)]()
[![11](https://img.shields.io/badge/macOS-11-red.svg)]()
[![12](https://img.shields.io/badge/macOS-12-blueviolet.svg)]()
[![download](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/jozews321/T430-Hackintosh-Opencore/releases/latest)
<img align="left" src="/resources/thinkpad-T430-EOL_79fd729b-5a2f-4f6f-b54b-ad10c1796a84.png.webp" alt="Lenovo Thinkpad T430" width="300">
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
- Intel Core i3 3110m or better (Celeron and Pentium CPUs are not supported)
- Integrated Intel HD4000 graphics (NVIDIA discrete GPUs are not supported)
- At least 4GB RAM 
- Stock Intel Centrino WiFI card (optional)
- Internal Broadcom BCM20702 Bluetooth 4.2 card (optional)
- Integrated Fingerprint reader and WWAN card are not supported
- Dual Booting is discouraged in this guide as there is many possibilities to cover here
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
| Processor           | Intel Core i5 3320M                           |
| Memory              | 8GB DDR3 1600MHz in Dual-Channel              |
| SSD                 | Intel 520 Series SSD 180GB                    |
| Graphics            | Intel HD Graphics 4000                        |
| Display             | 15.6" 1600x900                                |
| Audio               | Realtek ALC269VC                              |
| Ethernet            | Intel 82579LM Gigabit Network                 |
| WIFI                | Intel Centrino Ultimate N-6300                |
| Bluetooth           | Integrated Broadcom BCM20702 Bluetooth 4.2    |
  
</details>


## INSTALL GUIDE

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
<summary><strong>Generate SMBIOS serial numbers</strong></summary>
<br />
In this step you will generate the Serial, MLB, UUID and ROM to the config.plist (you will need to have ProperTree installed)

- Download the latest release [T430 EFI](https://github.com/jozews321/T430-Hackintosh-Opencore/releases/latest)
  <br /> <br /> 
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
- Write MacBookPro12,1
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
<summary><strong>macOS 12 Monterey - specific config</strong></summary>
<br />
Apple dropped support for the HD 4000 graphics so we need to get the config.plist ready for a system patch that we will get later in the post install

- Open config.plist

- Go to Misc/Security, find the entry SecureBootModel and set it to `Disabled`

- Go to NVRAM/Add/7C436110-AB2A-4BBB-A880-FE41995C9F82, find csr-active-config and set it to `030A0000`
  
Now your SIP config is ready for the System patch 
 
</details> 

<details>
<summary><strong>macOS 10.13 to 10.15 - specific config</strong></summary>
<br /> 
OC 0.7.2 changed the Default value for SecureBootModel to j137 to x86legacy thanks to this we won't be able to boot versions prior to macOS 11, but we can change it manually 

- Open config.plist

- Go to Misc/Security, find the entry SecureBootModel and set it to `j137`
  
Now your SBM is ready to boot 10.13 to 10.15 
 
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

- Install as normal
	
You can consult [OpenCore Guide - Installation Process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html) to get some instructions if you need them.
</details> 

## POST INSTALL
Different things that you might need to do after macOS is correctly installed

<details>
<summary><strong>macOS 12 Monterey - HD 4000 system patch</strong></summary>
<br /> 
Apple dropped the HD 4000 iGPU with macOS 12. If you dont install this you won't have any kind of graphics acceleration and your macOS 12 experience will be completely miserable

- Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases/tag/0.3.1) TUI version offline or online

- Run OLCP
	
- Choose volume root patch and follow instructions
	
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
* Acidanthera (OpenCore, VirtualSMC, Lilu, WhateverGreen and a lot more)
* Dortania (Opencore Install Guide, Opencore Legacy Patcher)
* OpenIntelWireless (Airportitlwm)
* [banhbaoxamlan](https://github.com/banhbaoxamlan/X230-Hackintosh) (X230 ACPI fixes)
* [5T33Z0](https://github.com/5T33Z0/Lenovo-T530-Hackinosh-OpenCore) (T530 ACPI fixes)
* [zhen-zen](https://github.com/zhen-zen/YogaSMC) (YogaSMC)











 











