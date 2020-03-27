# rdp2tcp

## Patching rdesktop

Download the 1.6.0 source code from: 

https://github.com/rdesktop/rdesktop/releases/tag/v1.6.0

Download the oop.patch

https://sourceforge.net/p/rdesktop/patches/107

Then patch : `patch < oop.patch`

Here are the packages needed for the compilation

- gcc, make, autoconf, automake, libssl1.0-dev

Compile with : `./bootstrap`

```
./configure
./make
```

## Compile rdp2tcp client

Go on the folder rdp2tcp > client

`./make`

it will compile the rdp2tcp client pluggin to use with the patched rdesktop

## Compile the rdp2tcp server

It is necesary to compile the rdp2tcp server for windows (it will be executed on the windows server). Then on a linux machine, use mingw32. 

```
make -f Makefile.mingw32
```

You will have to edit the makefile if you have an error with the mingw not found : CC=i686-w64-mingw32-gcc

## Usage

Firstly, you have to drop the rdp2tcp.exe server, on the Windows server where you have the RDP access.

With your client linux: `./rdesktop -r addin:rdp2tcp:/root/Documents/rdp2tcp/client/rdp2tcp`

Login and execute your rdp2tcp.exe server binary on your rdesktop session. 

## links

https://rdp2tcp.sourceforge.net/

https://connect.ed-diamond.com/MISC/MISC-055/Intrusion-depuis-un-environnement-Terminal-Server-avec-rdp2tcp

