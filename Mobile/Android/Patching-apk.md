# Méthodologie Android

## Repackaging d'une apk
### Décompilation et recompilation de l'apk
Decompiler l'apk : 

    $ apktool d myapp.apk -o extractedFolder

Récupérer la bonne version de frida sur : https://github.com/frida/frida/releases/

Copier/Coller la librairie frida dans le dossier /lib de l'apk :

    $ cp frida_libs/armeabi/frida-gadget-9.1.26-android-arm.so extractedFolder/lib/armeabi/libfrida.so

Ajouter le code suivant dans la méthode onCreate de la MainActivity.  Pour trouver la Man activity regarder dans le Manifest, puis diriger vous dans le fichier source smali en question et éditer le :

    const-string v0, "frida"
    invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V

Ajouter la permission internet dans le Manifest si elle n'y est pas présente :

    <uses-permission android:name="android.permission.INTERNET" />

Recompiler l'apk

    $ apktool b -o repackaged.apk extractedFolder/

En cas d'erreur supprimer le fichier 1.apk, le chemin est affiché dans les messages d'erreurs, puis recompiler :

```
$ rm ~/apktool/framework/1.apk
```

### Signature de  l'apk
Si vous n'avez pas de keystore, créez-en une :

    $ keytool -genkey -v -keystore custom.keystore -alias mykeyaliasname -keyalg RSA -keysize 2048 -validity 1000

(Saisissez un password)

> TODO : Logiciel de gestion de keystore

Signer l'apk :

    $ jarsigner -sigalg SHA1withRSA -digestalg SHA1 -keystore custom.keystore -storepass {mystorepass} repackaged.apk mykeyaliasname
    $ jarsigner -verify repackaged.apk

zipalign : 

    zipalign 4 repackaged.apk repackaged-aligned.apk

## Test de l'apk repackagé

    $ adb forward tcp:27042 tcp:27042

(désinstaller l'apk si elle est déjà installée)

    $adb install repackaged-aligned.apk

Lancer l'apk, un écran blan apparaitra

Sur votre ordinateur lancer là commande suivante pour démarrer l'apk : 

    $ frida -H 127.0.0.1 Gadget

