# Drozer Cheatsheet

Une fois l'agent installé et lancé sur l'android, taper les commandes suivantes sur le poste du pentester afin de s'y connecter :

`./adb forward tcp:31415 tcp:31415`
`drozer console connect`


## Exemple de commande

 - list : pour lister les packages installés
 - run app.package.list -f "string" : pour trouver les packages qui contiennent la "string"
 - run app.package.attacksurface com.monpackage.pwn : pour identifier la surface d'attaque
 - run app.package.manifest com.monpackage.pwn : extraire le AndroidManifest.xml
 - run app.activity.info -a com.monpackage.pwn : liste les activités exportées
 - run app.package.info -a com.monpackage.pwn : information basique
