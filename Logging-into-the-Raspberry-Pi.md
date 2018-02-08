There are several ways that you can login to the raspberry in order to get a command prompt.

# Username and password

The default username and password shipped with the image is a username of `pi` and a password of `raspberry`

# Method 1 - Use a USB keyboard and HDMI monitor

If you plug in an HDMI monitor into the HDMI port on the Raspberry Pi and plug in a USB keybpoard, and reboot the pi. You should see bootup messages and eventually see a login prompt. Enter the username and password as above and you'll be at a bash prompt in the /home/pi directory.

# Method 2 - Use the serial console

Connect a USB-to-TTL serial cable to the GPIO header pins. Adafruit sells a suitable [USB to TTL serial cable](https://www.adafruit.com/product/954) and it is connected as pictured:
![Serial Console Connections](https://github.com/mozilla-iot/wiki/raw/master/Photos/Serial-Console.jpg)
The black wire is ground and connects to pin 6 (GROUND). The white wire is RX and connects to pin 8 (UART TX). The black wire is TX and connects to pin 10 (UART RX). Leave the red wire (power) unconnected. Refer to the adafruit page for links to drivers for Windows and Mac. Fire up your favorite terminal program at 115200 baud, no flow control and you should see boot messages and a login prompt.

The image that we distribute has the serial console enabled by default.

# Method 3 - Use SSH

Mount the sdcard from the Raspberry Pi in your host computer and create an emtpy file called ssh (no extension, all lowercase) in the root of the boot partition. There are 2 partitions on the sdcard. The first partition is the boot partition and is formatted using FAT, so it should be mountable under Windows, Mac, and Linux. The second partition is EXT4 and is only mountable under linux.

Properly eject the sdcard and then boot up the Raspberry Pi. It will detect the ssh file and enable the ssh server. Login to the Raspberry Pi using:
```
ssh pi@gateway.local
```
and provide a password of `raspberry`.