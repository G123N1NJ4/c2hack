# ADB over Wifi

On your local computer write:

```
$ adb tcpip 5555 (with the phone connected on the laptop)
$ adb connect <ip_address>:5555
```

Then for doing frida over wifi over adb:
```
$ adb forward tcp:27042 tcp:27042
```

