This has been done on the Arlec PC190HA, but should work on other models and may be helpful for other Tasmota based devices.

# 1 Flash and configure the device
Follow the instructions here: https://github.com/ct-Open-Source/tuya-convert.
Using the included firmware should do the job.

Here is a brief summary of those instructions, modified for this application:
>Use a Linux machine. It can be a raspberry pi, just use a different SD card if you're using the one for your WebThings gateway.
>Enter the following commands to clone the repository and install the requirements:
>```
>git clone https://github.com/ct-Open-Source/tuya-convert
>cd tuya-convert
>./install_prereq.sh
>```
>Then start the flashing process by entering:
>```
>./start_flash.sh
>```
>Follow the directions provided. Choose the option to flash the tasmota.bin when appropriate.
>
>When that's complete you will be able to connect to a new tasmota wifi, and in your browser connect to http://192.168.4.1. There you can enter in your wifi ssid and password for the network with you WebThings Gateway. When it restarts it should be connected to your wifi, but you will need to be able to get a list of connected devices to be able to find it and connect to it.

Once you've found the device and connected to that IP, you will need to set up its functionality. You can do that by applying the template from here: https://templates.blakadder.com/arlec_PC190HA.html

Go to Configuration>Configure Other and enter in the template:
```
{"NAME":"Arlec-PC190HA","GPIO":[0,0,0,0,0,0,0,0,21,56,17,0,0],"FLAG":0,"BASE":18}
```
Make sure the Activate checkbox is checked, then save.

Finally, this has not helped for me, but the [Tasmota WebThings Adapter](https://github.com/tim-hellhake/tasmota-adapter#readme) directs you to go to the console and enter:
```
SetOption55 ON
```
In order to make it discoverable

# 2 Setup in the WebThings gateway

Install the adapter by going to Settings>Add-ons in the gateway and hit the '+' button. Find Tasmota in the list (by Tim Helhake) and add it.

At this point I have not found the device to be naturally discoverable so to add it in you can go back to Settings>Add-ons and configure the Tasmota adapter. Then hit the '+' at the bottom and enter in the device IP (only numbers and dots, don't include http://) and port 80. Once you apply that, the plug should appear when you go to add new devices.
