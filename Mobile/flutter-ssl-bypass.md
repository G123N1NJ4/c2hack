# Bypass SSL pinning on flutter app

## The MITM

The HTTPClient used by flutter by default does sent directly request to the target server and ignore proxy settings.

- Proxydroid
- iptable

```
sudo iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 443 -j REDIRECT --to-port 8080
sudo iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE
```

## Android

Unzip the app and then search the flutter.so library and then search for the  **session_verify_cert_chain**  method:

```
ARMv7 (32 bit)
binwalk -R "\x2d\xe9\xf0\x4f\xa3\xb0\x81\x46\x50\x20\x10\x70" ./libflutter.so

ARMv8 (64 bit)
binwalk -R "\xff\x03\x05\xd1\xfc\x6b\x0f\xa9\xf9\x63\x10\xa9\xf7\x5b\x11\xa9\xf5\x53\x12\xa9\xf3\x7b\x13\xa9\x08\x0a\x80\x52" libflutter.so
```

Then use the Frida script to hook the function:

```
function hook_ssl_verify_result(address)
{
 Interceptor.attach(address, {
  onEnter: function(args) {
   console.log("Disabling SSL validation")
  },
  onLeave: function(retval)
  {
   console.log("Retval: " + retval)
   retval.replace(0x1); 
  }
 });
}
function disablePinning(){
  // Change the offset on the line below with the binwalk result
  // If you are on 32 bit, add 1 to the offset to indicate it is a THUMB function: .add(0x1)
  // Otherwise, you will get 'Error: unable to intercept function at ......; please file a bug'
  var address = Module.findBaseAddress('libflutter.so').add(0x55a4ec)
  hook_ssl_verify_result(address);
}
setTimeout(disablePinning, 1000)
```

## iOS

You have to find the Flutter binary on the IPA:

```
unzip flutterapp.ipa 
file Payload/Runner.app/Frameworks/Flutter.framework/Flutter
lipo -thin arm64 Flutter -output FlutterThin
file FlutterThin
```

Then find the **session_verify_cert_chain** method

The first bytes of this method are:

```
ff 03 05 d1 fc 6f 0f a9 f8 5f 10 a9 f6 57 11 a9 f4 4f 12 a9 fd 7b 13 a9 fd c3 04 91 08 0a 80 52
```

You can search it with binwalk:

```
binwalk -R "\xff\x03\x05\xd1\xfc\x6f\x0f\xa9\xf8\x5f\x10\xa9\xf6\x57\x11\xa9\xf4\x4f\x12\xa9\xfd\x7b\x13\xa9\xfd\xc3\x04\x91\x08\x0a\x80\x52"
```

Once the address is identified you can use the frida script to bypass the ssl pinning:

```
function hook_ssl_verify_result(address)
{
  Interceptor.attach(address, {
    onEnter: function(args) {
      console.log("Disabling SSL validation")
    },
    onLeave: function(retval)
    {
      retval.replace(0x1);
    }
  });
}
function disablePinning()
{
  var m = Process.findModuleByName("Flutter"); 
  hook_ssl_verify_result(m.base.add(0x4068C8))
}
setTimeout(disablePinning, 1000) 
```

Otherwhise, it is also possible to use Frida to search the function address:

```
function hook_ssl_verify_result(address)
{
  Interceptor.attach(address, {
    onEnter: function(args) {
      console.log("Disabling SSL validation")
    },
    onLeave: function(retval)
    {
      retval.replace(0x1);
    }
  });
}
function disablePinning()
{
  var pattern = "ff 03 05 d1 fc 6f 0f a9 f8 5f 10 a9 f6 57 11 a9 f4 4f 12 a9 fd 7b 13 a9 fd c3 04 91 08 0a 80 52"
  Process.enumerateRangesSync('r-x').filter(function (m) 
  {
    if (m.file) return m.file.path.indexOf('Flutter') > -1;
    return false;
  }).forEach(function(r) 
  {
    Memory.scanSync(r.base, r.size, pattern).forEach(function (match) {
    console.log('[+] ssl_verify_result found at: ' + match.address.toString());
    hook_ssl_verify_result(match.address);
    });
  });
}
setTimeout(disablePinning, 1000) 
```

## Links

https://blog.nviso.eu/2020/06/12/intercepting-flutter-traffic-on-ios/

https://blog.nviso.eu/2020/05/20/intercepting-flutter-traffic-on-android-x64/

https://blog.nviso.eu/2019/08/13/intercepting-traffic-from-android-flutter-applications/