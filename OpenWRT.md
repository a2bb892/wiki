# Building the OpenWRT image with the gateway

To build an OpenWRT image for the Raspberry Pi, follow the directions on [this page](https://github.com/openwrt/packages/tree/master/lang/node-mozilla-iot-gateway).

NOTE: when you run menuconfig, ensure that the node-mozilla-iot-gateway has a star beside it to include the gateway in the image being built. Use the M if installing to run from an USB stick.

# Update the gateway version

Edit the file feeds/packages/lang/node-mozilla-iot-gateway/Makefile and change following:
* PKG_VERSION - which should match the gateway version
* PKG_RELEASE - start at 1 and increment if making small changes
* PKG_REV - should be the git hash from https://github.com/mozilla-iot/gateway

Rerun `make`

# Miscellaneous notes about using OpenWRT

These are some common things that I needed to do to use the gateway on my network.

## Enabling DHCP Client (for LAN)
```
  uci set network.lan.proto=dhcp
  uci commit
  /etc/init.d/network restart
```
## Enabling Wireless
```
  uci set wireless.radio0.disabled=0
  uci set wireless.default_radio0.ssid='OpenWrt'
  uci commit
  /etc/init.d/network restart
```
## Enabling WPA2
```
  uci set wireless.@wifi-iface[0].encryption=psk2
  uci set wireless.@wifi-iface[0].key="1234567890"
  uci commit wireless
  wifi
```
## Disabling DHCP Server (for WAN)
```
  /etc/init.d/odhcpd disable
  /etc/init.d/odhcpd stop
```

## Opening ports on the firwall

By default, OpenWRT doesn't allow access from the WAN side, but when debugging things and your OpenWRT router's WAN is on your local LAN, then it's useful to open some ports. The following script will do that:
```
#!/bin/sh

# allows ssh and http on the WAN side

uci add firewall rule
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='22'
uci set firewall.@rule[-1].name='allow-ssh-22'

uci add firewall rule
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='80'
uci set firewall.@rule[-1].name='allow-http-80'

uci add firewall rule
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='8080'
uci set firewall.@rule[-1].name='allow-http-8080'

uci add firewall rule
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='443'
uci set firewall.@rule[-1].name='allow-https-443'

uci add firewall rule
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='4443'
uci set firewall.@rule[-1].name='allow-https-4443'

uci commit firewall
/etc/init.d/firewall restart
```