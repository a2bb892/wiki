## Install Linux

We recommend Raspbian Lite, but you can also use Raspbian or another Linux. The instructions below are for Debian-based systems like Raspbian.

Options include:
  * Buy a microSD card with NOOBS pre-installed and use it to install Raspbian Lite
  * Download and flash [NOOBS](https://www.raspberrypi.org/downloads/noobs/) to a microSD card
  * Download and flash [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) Lite to a microSD card

Note: If you install via NOOBS, it's easiest to log into your WiFi network from NOOBS before installing Raspbian as it automatically passes over the configuration.

## Boot & Login

Boot the Raspberry Pi (with a keyboard and display plugged in) and login

* Username: `pi`
* Password: `raspberry`

## Set up WiFi (Optional)

(If you already set up WiFi using NOOBS or you're plugging in an Ethernet cable you can skip this step)

`$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

Add a configuration as below, replacing "my-ssid" and "my-network-key" with your WiFi credentials:

```
network {
  ssid="my-ssid"
  psk="my-network-key"
}
```

At this point, wpa-supplicant will normally notice a change has occurred within a few seconds, and it will try and connect to the network. If it does not, either manually restart the interface with `sudo ifdown wlan0` and `sudo ifup wlan0`, or reboot your Raspberry Pi with `sudo reboot`.

You can verify if it has successfully connected using `ifconfig wlan0`. If the inet addr field has an address beside it, the Pi has connected to the network. If not, check your password and SSID are correct. 

## Enable SSH Server (Optional)

This will allow you to log into the Pi remotely so you won't need to keep it plugged into a display and keyboard.

`$ sudo raspi-config`

You should first change the default password (you can also change the hostname if you wish).

Go to Interfacing Options and select SSH to enable the SSH server.

([more info](https://www.raspberrypi.org/documentation/remote-access/ssh/)).

## Static IP Address (Optional)

You should be able to log into the pi using the hostname raspberrypi.local (e.g. `ssh pi@raspberrypi.local`), or by finding its dynamically assigned ip address by typing `ifconfig`.

Alternatively you might want to set a static IP address.

Add the following to the top of `/etc/dhcpcd.conf` setting your desired IP address and setting `routers` & `domain_name_servers` to the IP address of your router. Replace `wlan0` with `eth0` if using Ethernet rather than WiFi.

```
interface wlan0
static ip_address=192.168.1.42
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

Reboot:

`$ sudo reboot`

