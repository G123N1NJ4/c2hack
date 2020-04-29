# Analyze JNI methods from library


## Radare2
```
r2 -A lib.so
afl
pdf @sym.Java_com_Interesting_function
```

```
aaa
iE (find lib exports)
s 0x0000XXX (seek to address function)
pdg (ghidra plugin for speudo code decompilation)
```

## Local execution and debug with gdb

If you are looking for a native function wich is loaded from a library like:
```
static {
    System.loadLibrary("Hacklib");
}
```
On a file like: ./com.toto.tata/settings/SettingsManager.java

Create an Android project with Android Studio. 
Be careful, it is important to use the same package name of your app "com.toto.taa"

 - In activity_main.xml, add a button and set its id to "btnFlux"
 - Now in MainActivity.java edit the onCreate method with the following code:
```
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
    
Button btn = findViewById(R.id.btnFlux);

btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view)
    {
        // Do something 
    }
});
```
Now link the .so library. The .so file need to be on a structure like:

```
App
   build
   src
     androidTeste
     main
	java
	  com.toto.tata
	    settings
            MainActivity
        jniLibs
	  x86_64
             hacklib.so
```

After that, create a package named settings inside of com.toto.tata, and create a class named SettingsManager. Edit the created class with the following code:

```
package com.too.tata.settings;

import java.util.Map;

public class SettingsManager {

    public static native String flux(Map<String, String> map);

    static {
        System.loadLibrary("hacklib");
 
```

Now edit the MainActivity.java in order to add the button listener:
```
HashMap<String, String> lol = new HashMap<>();
lol.put("parama", "valuea");
String ret = SettingsManager.flux(lol);
Log.i("REVERSING", ret);
```

Regarding the symbols, it is necessary to copy the mobile libraries:
```
$ mkdir fs
$ cd fs
$ mkdir -p sysroot/system
$ adb pull /system/lib sysroot/system/
/system/lib/: 313 files pulled. 44.8 MB/s (140367100 bytes in 2.988s)
$ adb pull /system/bin sysroot/system
/system/bin/: 299 files pulled. 44.3 MB/s (79616683 bytes in 1.712s)
$ chmod 755 sysroot/system/bin/*
```

Then add the lib

```
cp <path>/libhack.so sysroot/system/lib/
```

Enable the port forwarding in order to connect gdb to the process:

```
adb forward tcp:12345 tcp:12345
```

Launch gdb:
```
gdb -q
gdb-peda$ set sysroot sysroot
gdb-peda$ set solib-search-path sysroot/system/lib/
gdb-peda$ handle SIG33 nostop noprint

gdb-peda$ file sysroot/system/bin/app_process
```

On the mobile, make sure to launch the custom app, then launch gdb server
```
$ adb shell
# gdbserver64 --attach :12345 10941
```

On your laptop
```
gdb-peda$ target remote :12345

b Java_com_byeline_hackex_settings_SettingsManager_flux
c
vmmap libhack.so
disas Java_com_byeline_hackex_settings_SettingsManager_flux
```

## Hook JNI native methods with Frida

In order to list the functions present on the lib it is possible to use the following command:

```
nm --demangle --dynamic lib.so
```

Once you have find the function that you want to hook. There is 2 approach:

- Hook on the Java layer (intercept the java calls to the JNI);
- Dive to the implementation of the function in C.

## Hook with the Java api

```javascript
Java.perform(function () {
  // we create a javascript wrapper for MainActivity
  var Activity = Java.use('com.toto.jniapp.MainActivity');
  // replace the Jniint implementation
  Activity.functionToHook.implementation = function () {
    // console.log is used to report information back to us
    console.log("Inside functionToHook now...");
		//code...
  };
});
```

## Hook the native C implementation

```javascript
Interceptor.attach(Module.getExportByName('lib.so', 'functionToHook'), {
    onEnter: function(args) {
    },
    onLeave: function(retval) {
      // simply replace the value to be returned with 0
      retval.replace(0);
    }
});
```

## Links

https://0x00sec.org/t/reversing-hackex-an-android-game/16243

https://erev0s.com/blog/how-hook-android-native-methods-frida-noob-friendly/