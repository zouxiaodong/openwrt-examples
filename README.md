### Introduction

This project contains mutliple examples to show how to package an application for [LEDE](https://lede-project.org).

Feel free to submit new examples and fix errors! :-)

myapp1:
* dummy daemon written C that forks into background and exits after 60 seconds
* the following files will be installed:
  * /usr/bin/myapp1
  * /etc/config/myapp1
  * /etc/init.d/myapp1
* cmake build system
* source code is part of the package

myapp2
* dummy program written in C that just prints out a message
* the following files will be installed:
  * /usr/bin/myapp2
* make build system
* source code is part of the package
* package let you select features called "foo" and "bar"

These examples are licensed under CC0-1.0 / placed into Public Domain.

Official OpenWrt/LEDE packages can be found here and are a good reference:
* https://github.com/openwrt/packages
* https://github.com/openwrt-routing/packages

### Build Images and Packages

These are the instructions to build an image
for your router including the example applications:

```
git clone git://git.lede-project.org/source.git
cd source

./scripts/feeds update -a
./scripts/feeds install -a

git clone https://github.com/mwarning/lede-examples.git
cp -rf lede-examples/myapp* package/
rm -rf lede-examples/

make defconfig
make menuconfig
```

Now select the right "Target System" and "Target Profile" for your target device.
Also select the examples you like to build:

* "Utilities" => "myapp1"
* "Network" => "VPN" => "myapp2"

Packages are selected when there is a <*> in front of the name (hit the sapce bar twice).

Finally - build the image:
```
make
```

You can now flash your router using the correct image file inside ./bin/targets/. The images usually contain all build packages already.
The single *.ipk packages are located in ./bin/packages, in case you want to install them on other devices.

To install/update a package, transfer the ipk file to your target device to /tmp/ using scp.
The package can then be installed calling e.g. `opkg install myapp-0.1-1.ipk`.

### Test packages

To test your program you need to login into your router (telnet or ssh).

* myapp1:
You can execute `myapp1` or `/etc/init.d/myapp1 start` on the console.
The application will show a short message before it disconnects
from the console and runs for 60 seconds as background process.

* myapp2
`myapp2`on the console will just start that program.
