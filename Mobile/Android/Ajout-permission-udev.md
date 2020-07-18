Add: /etc/udev/rules.d/51-android.rules

Then append the line for your mobile:  

Nexus 7: `SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", MODE="0666", GROUP="plugdev"`

