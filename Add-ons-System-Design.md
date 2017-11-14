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
* Bundled into Gateway and disabled by default

# Authentication Options
* Add-on specific JWT from `JSONWebToken.issueToken(-1)`
* OAuth2 confidential client (unimplemented, JWT would expire and need to be renewed, user could manage the scope and permissions of add-ons)

# APIs
* Adapter API (mirroring the Web Thing WebSocket API over IPC)
* Local HTTP/WS API: Talks to the Gateway through `node-fetch` and `ws` libraries. Currently in use by the Rules Engine.

# Open Questions
* Do we actually want to provide user installable add-ons and the security risks that brings, or should all add-ons have to be added at build time?

# Proposed Add-on Design

## Package Layout

An adapter or plugin should be an npm-compatible package, such that it has one top-level directory, corresponding to the package name, i.e. `moziot-adapter-mock`.

### `package.json`

Inside the top-level package directory should be a `package.json` file. It must be a valid npm package, with an additional field, `moziot`, as such:

```
{
  "name": "moziot-adapter-gpio",
  "version": "0.2.1",
  "description": "GPIO adapter plugin for Mozilla IoT Gateway",
  "main": "index.js",
  "keywords": [
    "moziot",
    "adapter",
    "gpio"
  ],
  "homepage": "https://iot.mozilla.org/",
  "license": "MPL-2.0",
  "repository": {
    "type": "git",
    "url": "https://github.com/mozilla-iot/gateway.git"
  },
  "bugs": {
    "url": "https://github.com/mozilla-iot/gateway/issues"
  },
  "moziot": {
    "api": {
      "min": 1,
      "max": 1
    },
    "plugin": false,
    "config": {
      "pins": {
        "18": {
          "name": "led",
          "direction": "out",
          "value": 0
        }
      }
    }
  }
}
```

The `moziot` fields are explained as follows:

- `api.min` - The minimum API version supported.
- `api.max` - The maximum API version supported.
- `plugin` - Whether or not this add-on is a plugin, meaning it runs in its own process.
- `config` - (Optional) Generic configuration object. When the user changes config options, they will be stored in the gateway's settings database, and will persist across package upgrades.