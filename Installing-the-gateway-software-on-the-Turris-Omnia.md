* Copy the gateway [medkit](https://s3-us-west-1.amazonaws.com/mozillagatewayimages/images/omnia-medkit-moziot-0.9.0-3.tar.gz) onto a USB thumb drive. There should only be one file on the thumb drive which starts with omnia-medkit.
* Insert the USB thumb drive into one of the USB ports on the Turris Omnia
* Press and hold the reset button until 4 LEDS (power, 0, 1, and 2) are lit. Release the reset button. I put a [youtube video](https://youtu.be/aSOtBaTHLk0) showing the entire process.
* It takes about 2 minutes for the image to be written
* It takes about 30 seconds for the new image to boot
* It takes about 2 minutes for the gateway to startup (it still runs webpack)
* You should then be able to connect to WebThings Gateway XXXX WiFi AP and browse to gateway.local (or 192.168.2.1) - currently the captive portal isn't working.

