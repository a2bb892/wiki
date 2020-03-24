# General

* [[Glossary of Terms]]
* [[Installing Node.js]]
* [[Setting up Raspberry Pi]]

# WebThings Gateway

* [REST & WebSocket API](https://iot.mozilla.org/wot/)
* [Schemas](https://iot.mozilla.org/schemas/) - used in thing descriptions provided by API
* [Getting Started Guide](https://iot.mozilla.org/docs/gateway-getting-started-guide.html)
* [User Guide](https://iot.mozilla.org/docs/gateway-user-guide.html)
* [[Gateway Architecture]] - The basic architecture of the system to help devs get started
* [[Supported Hardware]] - A list of supported gateway hardware, adapters and devices
* [[Logging into the Raspberry Pi]] - How to access the command line
* [Gateway Remote Access](https://github.com/mozilla-iot/registration_server/blob/master/doc/flow.md) - How secure remote access works
* [Configuring GPIO](./Configuring-GPIO-for-use-with-the-gpio-adapter) - How to configure General Purpose Input/Output Ports on Raspberry Pi
* [curl examples](https://github.com/mozilla-iot/curl-examples/) - A repository containing some example scripts which can login, get a list of things, and get or set properties.
* [[Fluent: Making a new translation]]

## Build and Release

* [Manually build the gateway](https://github.com/mozilla-iot/gateway#readme)

### Raspbian

#### The new way

This is the process as of 0.12. You can build on either Linux or macOS. You will need the following prerequisites:
* Docker
* (On Linux) qemu
* (On Linux) qemu-user-static
* (On Linux) binfmt-support
* (On Linux) you will need the `loop` kernel module to be loaded, e.g. `sudo modprobe loop`

After installing the packages above, run the following inside a cloned gateway repo.

```sh
cd <gateway>/image
./build.sh
```

Wait for quite a while. After the process completes, you'll be left with the 4 files required for a release:
* `gateway-<version>.img.zip` - the Raspbian image
* `gateway-<version>.img.zip.sha256sum` - sha256sum of the Raspbian image
* `gateway-<checksum>.tar.gz` - gateway tarball used for OTA updates
* `node_modules-<checksum>.tar.gz` - node_modules tarball used for OTA updates

To test the Raspbian image, just flash it to an SD card as you normally would. To test the OTA update, you'll need to upload the relevant files to a GitHub release and then follow [these instructions](./Testing-prerelease-OTA-updates).

#### The old way

The files required for this method don't exist past 0.11, but the instructions should be kept around just in case we need to switch back. It's a non-trivial process.

* [[Creating the base image file for the Raspberry Pi]]
* [Creating the final image](https://github.com/mozilla-iot/rpi-image-builder/blob/master/README.md)
* [[How To Release a Gateway OTA Update]]
* [[Testing prerelease OTA updates]]
* [[Loop mounting a Raspberry Pi image file under Linux]]

### OpenWrt (old)

* [[OpenWrt]]
* [[Setting up the Turris Omnia Router]]
* [[Installing the gateway software on the Turris Omnia]]

## Testing

* [[Running OAuth Locally]]
* [[HOWTO: Use the staging server for testing]]
* [[Test Gateway Instance]]

## Add-ons

### General

* [[HOWTO: Create an add-on]]
* [Add-on Guidelines](https://github.com/mozilla-iot/addon-list/blob/master/guidelines.md)
* Examples:
    * [Adapter](https://github.com/mozilla-iot/example-adapter)
    * [Notifier](https://github.com/mozilla-iot/example-notifier)
    * [Extension](https://github.com/mozilla-iot/example-extension)
* Add-on APIs:
    * [Node.js](https://github.com/mozilla-iot/gateway-addon-node)
    * [Python](https://github.com/mozilla-iot/gateway-addon-python)
* [[Add-on Configuration]]
* [Add-on Packaging](https://github.com/mozilla-iot/addon-list/blob/master/manifest.md)
* [Add-on Publishing](https://github.com/mozilla-iot/addon-list#readme)
* [[Adapter IPC]] - Inter-process communications between an add-on and the gateway
* [[Command Line Tool]] - Useful for debugging
* [[Using the debug controller]]

### Z-Wave

* [[Installing OpenZWave]]
* [[Node for OpenZWave]]
* [[Debugging OpenZWave]]

### Zigbee

* [[Debugging Zigbee]]
* [[Recording Frames sent by XCTU (Zigbee)]]
* [[Zigbee Attributes]]
* [[HOWTO: Factory reset a Cree Connected bulb]]
* [[HOWTO: Factory reset a Hue bulb]]
* [[HOWTO: Factory reset a Hue Wireless Dimmer]]
* [[HOWTO: Factory reset an IKEA bulb]]
* [[Pairing SmartThings sensors]]

# External Links and Resources

* [Mozilla Hacks](https://hacks.mozilla.org/category/web-of-things/) - official blog
* [twobraids](https://www.google.com/search?hl=en&q=site%3Awww.twobraids.com%20%22things%20gateway%22) - series of posts about the gateway itself and scripts to interact with it
* [YouTube](https://youtube.com) - search for "mozilla iot", "mozilla things", etc.

# Environment Setup

* [Setup of eslint in PyCharm](./PyCharm-Setup) - Get PyCharm to use the proper linting tool for the WebThings Gateway