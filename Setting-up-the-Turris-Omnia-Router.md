These are the steps I used to setup the Turris Omnia router.

My router was running Turris OS version 3.11.2 with kernel version 4.4.169-7bc33afbb1b35f5830b2b1b42c9cd8a0-2

# Initial Setup
From a factory reset (should be the same as fresh out of the box). If needed, [this page](https://docs.turris.cz/hw/omnia/rescue_modes/) describes how to perform a factory reset.
* Disconnect all ethernet cables
* Connect an ethernet cable from your computer or laptop to one of the LAN ports
* In your browser connect to 192.168.1.1. This connects you to the Foris web interface.
* Set a password, and under "Advanced administration" change from "Don't set this password" to one of the other settings. If you choose "Use Foris password" then the same password will be used for ssh'ing into the box as accessing the web interface.
* Click "Save Changes" and then "Next step".
* On the WAN page, I left everything at the defaults. Click "Save changes" and once the turris has reconnected, press "next step".
* Set your Region and timezone. I left the "Time settings" as "via ntp". Click "Save changes" and "next step".
* For DNS, I left the settings at the defaults. Click "Save changes" and "next step".
* For the Updater, I unchecked the extra languages (options) and left the other settings at their defaults. Click "Save changes" and "next step".
* Initial setup should now be complete.

# Setup Wireless
* Click on the WI-FI  link on the left.
* Check "Enable Wi-Fi 1"
* Enter an SSID and network password
* Click "Save changes"

# Setup as a Router
* Your Turris Omnia should now be setup as a Router. It will have a cable plugged into the WAN size which goes to the internet, and devices on wireless and the LAN ports will be assigned IP addresses in the 192.168.1.x network. The router will have an address of 192.168.1.1.

# Setup as an Access Point instead of a Router.
If you'd like to use the Turris Omnia as an access point (i.e. part of an existing network) then you can do the following.
* Click on "LAN" on the left side.
* Change the "LAN mode" from "Router" to "Computer"
* Assign a static IP address on your existing network (I think you could also use DHCP).
* If you're using a static IP address, fill in the gateway and DNS as appropriate for your network.
* Click "Save Changes"
* I then removed the ethernet cable from the laptop to the turris omnia, and plugged the turris omnia into my existing network via one of the LAN ports.
* Remove any WAN cable
* Connect your laptop to your existing network and you should be able to connect to the turris omnia using the static IP address you assigned above.

# SSH into your turris omnia
* use ssh root@static-ip-address and use the password you configured in the initial setup
* If you have an ssh-agent configured on your host machine you can setup passwordless ssh by running ssh-copy-id root@static-ip-address

# Update the package list
* From the ssh command line, run `opkg update`
* You can use `opkg list` to see the list of available packages. I usually combine this with grep.
* If you run `opkg list | grep moz` you should see node-mozilla-iot-gateway.
* Run `opkg install node-mozilla-iot-gateway`.
* You can now browse to http://static-ip-address:8080 to get to the gateway or you can browse to http://static-ip-address/ and it will show a list of 3 UIs and click on the gateway one. This particular redirection only seems to work for the initial setup. It seems to be setup to redirect to port 8080.

The gateway software is installed into /opt/mozilla-iot/gateway and the configuration directory is /.mozilla-iot

The log messages get logged into /var/log/messages. You can use `grep npm /var/log/messages` to get at the messages.

The gateway is managed using [procd](https://openwrt.org/docs/techref/procd) and the configuration file can be found in `/etc/init.d/mozilla-iot.gateway`