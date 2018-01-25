# Package Layout

An adapter or plugin should be an npm-compatible package, such that it has one top-level directory, named `package`.

## `package.json`

Inside the `package` directory should be a `package.json` file. It must be a valid npm package, with an additional field, `moziot`, as such:

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