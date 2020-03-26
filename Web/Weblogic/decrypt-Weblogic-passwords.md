# Decrypt Weblogic Passwords

If you have reached to access to : **boot.properties** and **config.xml**, you should want to decrypt them.

You need to have access and logged in to the respective unix server.

Go to Oracle bin directory, eg : "oracle_home/common/bin"

Execute the following script : `./wlst.sh`

After that you'll get the WLST prompt in offline mode : wls:/offline>. Invoke the following command : `wls:/offline> domain = "/opt/apps/user_projects/domains/domain_name"`

"/opt/apps/user_projects/domains/domain_name" is equivalent to the install directory of WebLogic, you may have to adapt it.

Now you can decrypt the password : 

```
wls:/offline> service = weblogic.security.internal.SerializedSystemIni.getEncryptionService(domain)
wls:/offline> encryption = weblogic.security.internal.encryption.ClearOrEncryptedService(service)
wls:/offline> print encryption.decrypt("{AES}WDhZb5/IP95P4eM8jwYITiZs01kawSeliV59aFog1jE=")
 weblogic123
```

## Links 
https://geekflare.com/decrypt-weblogic-password/
