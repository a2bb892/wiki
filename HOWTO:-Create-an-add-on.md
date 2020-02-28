This guide will help you create an add-on for the WebThings Gateway.

## High-Level Concepts

### Add-on

An add-on is a collection of code that the gateway runs to gain new features. This is loosely modeled after the add-on system in Firefox where each add-on adds to the functionality of your gateway in new and exciting ways. There are three primary classes of addons: adapter, notifier, and extension.

### Adapter Add-on

An adapter add-on adds support to the gateway for some new class of devices (either physical or virtual). An adapter add-on has three primary components: adapter, device, and property.

#### Adapter

An adapter object manages communication with a device or set of devices. This could be very granular, such as one adapter object communicating with one GPIO pin, or it could be much more broad, such as one adapter communicating with any number of devices over Wi-Fi. You decide!

#### Device

A device is just that, a hardware device, such as a smart plug, light bulb, or temperature sensor. It could also be a virtual device, such as a weather device that provides current weather conditions from a cloud service.

#### Property

A property is an individual property of a device, such as its on/off state, its energy usage, or its color.

### Notifier Add-on

A notifier add-on adds a new output block for the rules engine, allowing a user to be notified of some type of occurrence via some mechanism, e.g. email or SMS. A notifier add-on has two primary components: notifier and outlet.

#### Notifier

A notifier manages a set of notification outlets, which are presented to a user through the rules interface. A notifier could own and control any number of outlets.

#### Outlet

An outlet is an individual rule output, responsible for performing the actual notification process.

### Extension Add-on

An extension add-on adds new functionality to the gateway's web interface. This could be anything from applying a new stylesheet to creating entirely new panels and menu entries. An extension add-on has one primary component, an extension, and an optional API handler.

#### Extension

The extension object is what is actually loaded by the web interface at run-time, to provide the desired new functionality.

#### API Handler

An API handler is a back-end piece that can extend the gateway's REST API. This could allow for even more functionality from a UI extension than only what is provided by the gateway's API.

## Supported Languages

Add-ons have been written in Node.js and Python so far, and official JavaScript and Python bindings are available on the gateway platform. If you want to skip ahead, you can check out the list of [examples](#examples) now. However, you are free to develop an add-on in whatever language you choose, provided the following:

* Your add-on is [properly packaged](https://github.com/mozilla-iot/addon-list/blob/master/manifest.md).
* Your add-on package bundles all required dependencies that do not already exist on the gateway platform.
* If your package contains any binaries, they must be compiled for all target architectures and target operating systems. Officially supported are x64 for OSX and x64, arm and arm64 for linux. If your addon is only for the Raspberry Pi you should compile it for armv6l. All Raspberry Pi families are compatible with this architecture. The easiest way to do this would be to build your package on a Raspberry Pi 1/2/Zero, or to open a pull request to have it added to our [addon-builder](https://github.com/mozilla-iot/addon-builder).

## Implementation: The Nitty Gritty

### Evaluate Your Target Device

First, you need to think about the device(s) you’re trying to target.

* Will your add-on be communicating with one or many devices?
* How will the add-on communicate with the device(s)? Is a separate hardware dongle required?
    * For example, the Zigbee and Z-Wave adapters require a separate USB dongle to communicate with devices.
* What properties do these devices have?
* Are there existing [schemas](https://iot.mozilla.org/schemas/) you can advertise?
* Are there existing libraries you can use to talk to your device?
    * You'd be surprised by how many NPM modules, Python modules, C/C++ libraries, etc. exist for communicating with IoT devices.

The key here is to gain a strong understanding of the devices you’re trying to support.

### Start from an Example

The easiest way to start development is to start with one of the [existing add-ons](#examples) (listed further down). You can download, copy and paste, or git clone one of them into:
```
/home/pi/.mozilla-iot/addons/
```
Alternatively, you can do your development on a different machine. Just make sure you test on the Raspberry Pi.

After doing so, you should edit the `manifest.json` file as appropriate. In particular, the `id` field needs to match the name of the directory you just created.

### Coding

#### Adapter 

The key parts of the adapter add-on lifecycle are device creation and property updates. Device creation typically happens as part of a discovery process, whether that’s through [SSDP](https://en.wikipedia.org/wiki/Simple_Service_Discovery_Protocol), probing serial devices, or something else. After discovering the physical (or virtual) devices, you'll need to create Device objects, then build up their property lists. Make sure you handle property changes (that could be through events you get, or you may have to poll your devices). You also need to handle property updates from the user.

While it may be dated, [this blog post](https://hacks.mozilla.org/2018/02/making-a-clap-sensing-web-thing/) still provides an excellent walkthrough.

#### Notifier

The notifier lifecycle is fairly straightforward. As with an adapter, a notifier needs to start off by discovering the outlets it can create, or just do so statically based on some configuration. The outlets then primarily need to handle incoming notification requests.

#### Extension

Extension add-ons are much more open-ended, since they can do a wide variety of things. [This blog post](https://hacks.mozilla.org/2019/11/ui-extensions-webthings-gateway/) provides a good overview of building a basic extension.

### Testing

When you're ready to test, you'll need to restart the gateway process:
```
$ sudo systemctl restart mozilla-iot-gateway.service
```

After this restart, assuming you don't change your package ID, you can just reload the add-on from the UI by navigating to _Settings -> Add-ons_, then clicking "Disable" and then "Enable" on your add-on.

### Get Your Add-on Published!

Run `./package.sh` or whatever else you have to do to package up your add-on. Host the package somewhere, e.g. on GitHub as a release. Then, submit a pull request or issues to the addon-list repository.

### Notes

* The Python and Node.js add-on binding are automatically provided by the system in all of the gateway packages and images we maintain and distribute, so you do not need to include them yourself.
* Your add-on will run in a separate process and communicate with the gateway process via IPC over a WebSocket. That should hopefully be irrelevant to you, unless you're wanting to create an add-on in a language we don't already provide bindings for. The IPC protocol is documented [here](./Adapter-IPC).
* If your add-on process dies, it will automatically be restarted.

## Examples

The WebThings team, as well as our community, have built several add-ons that can serve as a good starting point and reference.

Node.js:
* Basic examples:
    * Adapter: https://github.com/mozilla-iot/example-adapter
    * Notifier: https://github.com/mozilla-iot/example-notifier
    * Extension: https://github.com/mozilla-iot/example-extension
* Virtual things adapter: https://github.com/mozilla-iot/virtual-things-adapter
    * Examples with no backing hardware, used to demonstrate lots of features of the add-on API as well as the different schemas which are available.
* Zigbee adapter: https://github.com/mozilla-iot/zigbee-adapter
* Z-Wave adapter: https://github.com/mozilla-iot/zwave-adapter
    * This is a very complex example, with native dependencies, as well as an additional library that gets built by the addon-builder.
* Philips Hue adapter: https://github.com/mozilla-iot/philips-hue-adapter
* GPIO adapter: https://github.com/mozilla-iot/gpio-adapter
* Timer adapter: https://github.com/tim-hellhake/timer-adapter
    * This is a simple example written in TypeScript.

Python:
* TP-Link adapter: https://github.com/mozilla-iot/tplink-adapter
* Eufy adapter: https://github.com/mozilla-iot/eufy-adapter
    * This add-on installs some native Python dependencies at runtime, rather than cross-compiling beforehand.

## References

Additional documentation, API references, etc., can be found here:

* Web Thing API specification: https://iot.mozilla.org/wot/
* Capability schemas: https://iot.mozilla.org/schemas/
* Adapter IPC API: https://github.com/mozilla-iot/wiki/wiki/Adapter-IPC
* Add-on guidelines: https://github.com/mozilla-iot/addon-list/blob/master/guidelines.md
* Add-on configuration: https://github.com/mozilla-iot/wiki/wiki/Add-on-Configuration
* Add-on packaging: https://github.com/mozilla-iot/addon-list/blob/master/manifest.md
* Add-on publishing: https://github.com/mozilla-iot/addon-list#readme
* Node.js add-on bindings: https://github.com/mozilla-iot/gateway-addon-node
* Python add-on bindings: https://github.com/mozilla-iot/gateway-addon-python