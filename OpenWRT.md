# Building the OpenWRT image with the gateway

To build an OpenWRT image for the Raspberry Pi, follow the directions on [this page](https://github.com/openwrt/packages/tree/master/lang/node-mozilla-iot-gateway).

NOTE: when you run menuconfig, ensure that the node-mozilla-iot-gateway has a star beside it to include the gateway in the image being built. Use the M if installing to run from an USB stick.

# Update the gateway version

Edit the file packages/lang/node-mozilla-iot-gateway/Makefile and change following:
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
