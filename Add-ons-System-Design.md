# User Stories
* As a developer I want to create an add-on which adds support for new protocols or devices
* As a developer I want to create an add-on which extends the UI of the gateway
* As a developer I want to package my add-on so that it can be distributed more easily
* As a developer I want to publish my add-on so that others can find it
* As a user I want to be able to discover available add-ons
* As a user I want to install an add-on
* As a user I want to configure an add-on
* As a user I want to un-install an adapter

# Add-on Types
* Adapter - Adds support for a new IoT protocol or device
* Extension - Extends the gateway UI

# Packaging Options
* Node module
* Zip file

# Manifest Options
* Extend npm package [package.json](https://docs.npmjs.com/files/package.json)
* Inspiration from Web Extensions [manifest.json](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json) (e.g. to add extra menu buttons).

# Distribution Options
* [npmjs.com](https://www.npmjs.com/)
* Add-ons directory at iot.mozilla.org

# Authentication Options
* Access a global JWT variable?
* OAuth?

# APIs
* Adapter API (mirroring the Web Thing WebSocket API over IPC)