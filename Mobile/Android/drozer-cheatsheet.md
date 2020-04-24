# Drozer Cheatsheet

## Installation

https://github.com/FSecureLABS/drozer

You should install:

- the drozer console on your laptop: `sudo dpkg -i drozer-2.x.x.deb`;
- the drozer apk on your mobile phone: `adb install drozer-agent-2.x.x.apk`.

## Connection

Once the agent installed on the mobile phone, launche the android app to test. Then enter the following commands on your laptop with the mobile phone connected in order to attach to drozer:

`./adb forward tcp:31415 tcp:31415`
`drozer console connect`


## Command usage

 - `list` : pour lister les packages installés
 - Package information
    - `run app.package.list -f "string"`: find the package with the following "string"
    - `run app.package.info -a "package name"` : list activities on the "package name" 
 - Extract manifest
    -  `run app.package.manifest com.monpackage.pwn`: extract the AndroidManifest.xml
 - Attack surface
    - `run app.package.attacksurface com.monpackage.pwn`
 - Exploiting activities
    - `run app.activity.info -a "package name" -u`
    - `run app.activity.start --component "package name" "component name"`
 - Exploiting Content provider
    - `run app.provider.info -a "package name"`
    - `run scanner.provider.finduris -a "package name"`
    - `run app.provider.query "URI"`
    - `run app.provider.update "URI" --selection "conditions" "selextion arg" "column" "data"`
    - `run scanner.provider.sqltables -a "package name"`
    - `run scanner.provider.injection -a "package name"`
    - `run scanner.provider.traversal -a "package name"`
 - Broadcast receivers
    - `run app.broadcast.info -a "package name"`
    - `run app.broadcast.send --component "packaage naame" "component name" --extra "type" "key" "value"`
    - `run app.broadcast.sniff --action "action"`
 - Exploiting service
    - `run app.service.info -a "package name"`
    - `run app.service.start --action "action" --component "package name" "component name"`
    - `run app.service.send "package name" "component name" --msg "what" "arg1" "arg2" --extra "type" "key" "value" --bundle-as-obj`

