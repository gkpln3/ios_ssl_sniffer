SSL iOS Logger
==============
This utility uses frida in order to sniff plain SSL traffic.
It is taken from google's logger https://github.com/google/ssl_logger which is ment to run locally.

Motified by Guy Kaplan to sniff the traffic of an external usb device.

Setup
-----

### On the iPhone ###
https://www.frida.re/docs/ios/#with-jailbreak
> Start `Cydia` and add Frida’s repository by going to `Manage` -> `Sources` -> `Edit` -> Add and enter `https://build.frida.re`. You should now be able to find and install the `Frida` package which lets Frida inject JavaScript into apps running on your iOS device. This happens over USB, so you will need to have your USB cable handy, though there’s no need to plug it in just yet.


### On Your PC ###
You have to install frida first.
```bash
pip install frida
```
**Note this only works with python27**

run the command with
```bash
python2 ssl_logger.py
```

Make sure your iDevice is connected using USB first.


Changes documentation
=====================
First, I changed the line where it was using `frida.attach` to connect to frida to get the device to attach using USB (can be changed to ssh).
Then, it appears that iOS does not use `libssl`, I discoverd that by using `frida-trace -i "*SSL*" -U` and saw that the symbools come from a library called `libboringssl`, so i changed the line where it said `libssl` to `libboringssl`, the library itself has a very similar API as `libssl`, but lacks the call to get FD, (it should be there somewhere, with some name, but it was irrelavant for my need as it was only being used to get IP addresses and ports which I already knew), so I removed the call to `SSL_get_fd` function.

Then the tweak was ready to run :)
