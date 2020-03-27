# Root One plus 6

## Unlock the bootloader
 - First go on the parameter application and activate the developer options by click 7 times on the build number
 - On the developer options, activate the OEM unlock, advanced reboot and usb debug.
 - On your laptop install fastboot and adb
 - On the phone, press the power button and select reboot then bootloader
 - Now with your laptop on a terminal: `fastboot devices` (add sudo if there is permissions problems)
 - If the command has shown a device its ok, otherwice... repeat the fastboot and adb install and good luck
 - Now unlock the phone:  `fastboot oem unlock`

## TWRP recovery mode
Download:
 - TWRP https://eu.dl.twrp.me/enchilada/twrp-3.2.3-1-enchilada.img.html
 - Magisk https://github.com/topjohnwu/Magisk (copy it on your phone)

 - Activate again the USB debug mode and the advanced reboot.
 - On the mobile phone, press the power button, reboot then bootloader
 - Load the twrp image: `fastboot boot twrp.img`
 - On the twrp, swipte to the right to allow modification, go to install and select the magisk.zip file to install.

## Link 
http://oneplus6.fr/root
