# How to install CA Burp on Oreo 

Firstly extract the Burp certificate.
Then transform the der file to pem:
```
openssl x509 -inform DER -in cacert.der -out cacert.pem
openssl x509 -inform PEM -subject_hash_old - in cacert.pem | head -1
mv cacert.pem <hash>.0
```

Now push the ca on the mobile phone:

```
adb push <hash>.0 /sdcard/
```

Remount the directory `/system` in order to access `/system/etc/security/cacerts/`.

```
adb shell
sudo su
mount -o rw,remount /system

mv /sdcard/<hash>.0 /system/etc/security/cacerts/
chmod 644 /system/etc/security/cacerts/<hash>.0
adb reboot
```