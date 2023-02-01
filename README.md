# Hackintosh Guide for **Asus FX506LHB**
**This guide it's updated to OpenCore 0.8.8 and NOT YET tested on a friend's laptop on Ventura.**

<!-- START shields -->
<div>
    <!-- downloads --><a href="https://github.com/RobyRew/ASUS-FX506LHB-Hackintosh_OpenCore/releases">
        <img src="https://img.shields.io/github/downloads/RobyRew/ASUS-FX506LHB-Hackintosh_OpenCore/total" alt="downloads"/>
    </a>
    <!-- version --><a href="https://github.com/RobyRew/ASUS-FX506LHB-Hackintosh_OpenCore/releases/latest">
        <img src="https://img.shields.io/github/release/RobyRew/ASUS-FX506LHB-Hackintosh_OpenCore.svg" alt="latest version"/>
    </a>
     <!-- platform --><a href="https://github.com/RobyRew/ASUS-FX506LHB-Hackintosh_OpenCore">
        <img src="https://img.shields.io/badge/platform-macOS-lightgrey.svg" alt="platform"/>
    </a>
</div>
</br>
<!-- END shields -->

![Asus FX506LHB running macOS Ventura](/Docs/Images/Asus-FX506LHB-macOS.png)

# Specifications
Here's the [Amazon Link](https://www.amazon.es/ASUS-TUF-Gaming-F15-FX506LHB/dp/B09TRDT8DC) for this exact model.

| Component | Name |
|:--- |:---|
| Motherboard:  | FX506LHB **HM470** |
| CPU: | **Intel** i5-10300H |
| RAM: | **Micron** 8ATF1G64HZ-3G2J1 16GB 1600MHz |
| iGPU: | **Intel** UHD 630 (Mobile) |
| dGPU: | **nVidia** GeForce GTX 1650 Ti (DISABLED) |
| NVMe: | **Micron** 2210_MTFDHBA512QFD |
| WiFi/BT: | **MediaTek** MT7921 (Type CNVi) |
| Audio: | **RealTek** ALC256 |
| Ethernet: | **RealTek** RTL8111 |
| Trackpad: | **ELAN1200** Precision TouchPad (Type HID) |
| Keyboard: | PS/2 Keyboard (ISO Spain)|
><strong>Note:</strong> The WiFi/BT Card needs to be replaced with a **BCM94352Z**  in order to have FULL Native Wirelless Features.

# Working Status
 - ### **Fully Working**
    - Built-In Display

    - Jack 3.5mm, Speakers & Microphone
    - DC-IN & Battery
    - NVMe & HDD
    - Camera
    - Ethernet
    - Keyboard
    - Trackpad

 - ### **Partially Working**
    - [All USB Ports](/Docs/Images/Guide/Asus-FX506LHB-layout.png) (Not mapped correctly YET)

- ### **Not Working**
    - dGPU *(nVidia GTX 1050Ti)*
    - WiFi
    - Bluetooth
    - HDMI Port
    - Continuity Features (Apple Ecosystem Fancy Things)

<details open>
    <summary><h3>&nbsp;&nbsp;🔍&nbsp;&nbsp;In-depth Info</h3></summary>

| Name | Solution |
|:--- |:--- |
| Built-In Display  | Works thanks to `SSDT-PNLF.aml` *(Backlight Fix)* and `WhateverGreen.kext` *(iGPU Fixes)*. |
| All USB Ports | Thanks to `USBMap.aml` *(Ports mapped for macOS)* and `SSDT-EC-USBX-LAPTOP.aml`. |
| Jack 3.5mm, Speakers & Microphone | Thanks to `AppleALC.kext` *(Audio Driver)* and layout setted to "1" (Dec) or "01000000" (Hex). |
| DC-IN & Battery| `VirtualSMC.kext` with `SMCBatteryManager.kext` allows management off charging and Battery Info. |
| NVMe & HDD | It used to be something tricky but having SATA on AHCI Mode on *BIOS/UEFI* and latest firmware for the NVMe is enough. |
| Camera | Is connected via Internal USB and it works. |
| Ethernet | Thanks to `RealtekRTL8111.kext` (Ethernet Driver). |
| Keyboard | Thanks to `VoodooPS2.kext` (Driver for PS2 devices). |
| Trackpad | Thanks to `SSDT-XOSI.aml`(Make some Windows Features Avaible for macOS too) and with `VoodooI2C.kext` in combination with `VoodooI2CHID.kext`(Driver for I2C and HID devices like my trackpad). |
| PowerOff, Reboot & Sleep | Working thanks to having correct configs for CPU, iGPU, disabled dGPU, mapped USB Ports and executed these commands after macOS installation: `sudo pmset autopoweroff 0`, `sudo pmset powernap 0`, `sudo pmset standby 0`, `sudo pmset proximitywake 0`, `sudo pmset tcpkeepalive 0`.|
| | |
| WiFi | The WiFi/BT Card needs to be replaced with a **BCM94352Z**  in order to have FULL Native Wirelless Features. |
| Bluetooth | The WiFi/BT Card needs to be replaced with a **BCM94352Z**  in order to have FULL Native Wirelless Features. |
| HDMI Port | HDMI may not work because of nVIDIA OPTIMUS dGPU+iGPU Combo. |
| Continuity Features | Nothing will without compatible WiFi-BT Card |
| | |
| dGPU | dGPU doesn't have any drivers on any macOS OS. |
</details>

---

# GUIDE OF INSTALLATION
<!-- BOOTABLE START -->
<details open>
<summary><h3>Making the Bootable USB</h3></summary>
    <h3>From macOS:</h3>
<p><a href="https://support.apple.com/en-us/HT201372"></a>Link to Apple's Guide</p>

**Download installers:** [Ventura](https://apps.apple.com/us/app/macos-ventura/id1638787999) - [Monterrey](https://apps.apple.com/es/app/macos-monterey/id1576738294) - [Big Sur](https://itunes.apple.com/us/app/macos-big-sur/id1526878132) - [Catalina](https://itunes.apple.com/us/app/macos-catalina/id1466841314) - [Mojave](https://itunes.apple.com/us/app/macos-mojave/id1398502828) - [High Sierra](https://itunes.apple.com/us/app/macos-high-sierra/id1246284741)

[(Alternative Download (Mr. Machintosh Post))](https://mrmacintosh.com/how-to-download-macos-catalina-mojave-or-high-sierra-full-installers/)

1. Connect a >=16 GB pendrive.
2. Open *Disk Utility* and Erase the USB with the name: *MyVolume*.
3. Open *Terminal* and use the proper commands for your macOS installer:
- Ventura: `sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`
- Monterrey: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`
- Big Sur: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`
- Catalina: `sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`
- Mojave: `sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`
- High Sierra: `sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

![Terminal](/Docs/Images/Guide/BootableUSB.png)

### From Windows:

[**Link to Dortania's Guide**](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html)

### From Linux:

[**Link to Dortania's Guide**](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html)

 ---
    
</details>
<!-- BOOTABLE END -->
<!-- BIOS START -->
<details open>
<summary><h3>BIOS Settings</h3></summary>
 
- Make Sure you have [Latest BIOS v323](https://www.asus.com/supportonly/FX504GE/HelpDesk_BIOS/)
- After Updating the BIOS, you just need to **DISABLE** `Secure Boot`, so don't worry about this part.
---
 
</details>

<!-- BIOS END -->

<!-- POST-INSTALL START-->

<details open>
  <summary><h3>Post Installation</h3></summary>
 
Open Terminal.app and run those commands:
~~~
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
~~~
>These 5 commands help fixing possible Sleep/Wake Issues
  
~~~
  - sudo rm /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist
  - sudo rm /Library/Preferences/SystemConfiguration/preferences.plist
~~~
>These 2 commands help fixing possible iCloud Ban/iMessages Ban or WiFi/Ethernet Issues (Use only if you want or need)

---

</details>
<!-- POST-INSTALL END -->


# Aditional Info

<!-- CREDITS START -->
<details open>
<summary><h3>Credits</h3></summary>

[Apple](https://apple.com) (macOS)

[OpenCore Team](https://github.com/acidanthera/OpenCorePkg) (Bootloader)

[Dortania](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html#starting-point) (Guide)

[@taarkov](https://github.com/taarkov) (Some ACPI ideas)

---

</details>
<!-- CREDITS END -->
If this guide has been useful for you, don't forget to give me a star ⭐️❤️
