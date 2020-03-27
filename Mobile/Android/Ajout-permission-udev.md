Créer le fichier : /etc/udev/rules.d/51-android.rules
Puis ajouter votre device 
Tablette nexus 7 : SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", MODE="0666", GROUP="plugdev"
