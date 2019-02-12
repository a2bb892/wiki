# Building the OpenWRT image with the gateway

To build an OpenWRT image for the Raspberry Pi, follow the directions on [this page](https://github.com/openwrt/packages/tree/master/lang/node-mozilla-iot-gateway).

NOTE: when you run menuconfig, ensure that the node-mozilla-iot-gateway has a star beside it to include the gateway in the image being built. Use the M if installing to run from an USB stick.

# Update the gateway version

Edit the file packages/lang/node-mozilla-iot-gateway/Makefile and change following:
* PKG_VERSION - which should match the gateway version
* PKG_RELEASE - start at 1 and increment if making small changes
* PKG_REV - should be the git hash from https://github.com/mozilla-iot/gateway

Rerun `make`

