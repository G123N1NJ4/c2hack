[+]Installation de iRET sur un iDevice:

-Se connecter en SSH sur le iDevice.
-Télécharger le fichier : wget –no-check-certificate https://www.veracode.com/sites/default/files/Resources/Tools/iRETTool.zip
-Déarchiver : unzip iRETTool.zip
-aller dans le répertoire : cd iRET-Tool
-installer le package : dpkg -i iRET.deb

[+]Maintenant, si l'icone de l'application n'apparait pas : il faudra installer les dépendances suivantes via cydia :

Python (2.5.1 or 2.7) (Need to be Cydia ‘Developer’)
coreutils
Erica Utilities
file
adv-cmds
Bourne-Again Shell
iOS Toolchain (coolstar version)
Darwin CC Tools (coolstar version)
An iOS SDK (presumably iOS 6.1 or 7.x) installed to $THEOS/sdks

[+]Pour installer Theos :
-Il faut avoir installé au préalable sur cydia le package : "BigBoss Recommended Tools"
-Ensuite via une connexion SSH sur l'iDevice taper la commande : installtheos3

[+]Pour installer dumpdecrypted
-Télécharger le zip sur un poste sous OSX : https://github.com/stefanesser/dumpdecrypted
-Compiler avec un mac (nécessite OSX pour compiler) en utilisant la commande : make
-Déposer le fichier crée : dumpdecrypted.dylib sous l'iDevice.

[+]Installer class-dump-z
-Se connecter en SSH à l'iDevice puis télécharger le package : wget --no-check-certificate https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/networkpx/class-dump-z_0.2-0.tar.gz
-Désarchiver : tar -xvzf class-dump-z_0.2-0.tar.gz 
-Copier le fichier class-dump-z dans iphone_armv6 :  cp class-dump-z /usr/bin/

[+]Fix Bug pour la sélection des applications
-Le path pour la liste des applications n'est plus d'actualité (à cause des modifications du iOS)
-Utiliser la touche : 'grep -R "/var/mobile/Applications" .' 
-Remplacer  par "/private/var/mobile/Containers/Bundle/Application/", dans tous les fichiers trouvé par la première commande grep
(Il semblerait que dl le repo venant de github sur le repo de masboq pourrait régler ces pbs).

[UPDATE] TODO : Le path à modifié à mon sens serait plus : /var/mobile/Containers/Bundle/Application/
--> En cours...

[+]Fix : /usr/bin/env: perl: No such file or directory
-Dans Cydia, Ajouter la repo : http://coolstar.org/publicrepo/
-Puis installer Perl.

[+]Si le server web ne répond pas, en étant connecté en SSH sur l'iDevice, aller dans  "/Applications/iRE.app" et ensuite taper : python iRE_Server.py


[+]source:
http://highaltitudehacks.com/2014/03/25/ios-application-security-part-32-automating-tasks-with-ios-reverse-engineering-toolkit-iret/
http://www.securified.in/ios-pentesting/ios-reverse-engineering-toolkitiret-installation/
http://www.toolswatch.org/2014/08/new-tool-ios-reverse-engineering-toolkit-iret-v1-0-released/
https://bugwrangler.in/2015/07/how-to-install-class-dump-z-on-your-ios-device/
https://github.com/masbog/iRET

