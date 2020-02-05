# Package Layout

An adapter or plugin should be an npm-compatible package, such that it has one top-level directory, named `package`.

## `package.json`

Inside the `package` directory should be a `package.json` file. It must be a valid npm package, with an additional field, `moziot`, as such:

```
{
  "name": "gpio-adapter",
  "display_name": "GPIO",
  "version": "0.3.3",
  "description": "GPIO adapter plugin for Mozilla IoT Gateway",
  "author": "Mozilla IoT",
  "main": "index.js",
  "keywords": [
    "mozilla",
    "iot",
    "adapter",
    "gpio"
  ],
  "homepage": "https://github.com/mozilla-iot/gpio-adapter",
  "license": "MPL-2.0",
  "repository": {
    "type": "git",
    "url": "https://github.com/mozilla-iot/gpio-adapter.git"
  },
  "bugs": {
    "url": "https://github.com/mozilla-iot/gpio-adapter/issues"
  },
  "files": [
    "LICENSE",
    "SHA256SUMS",
    "gpio-adapter.js",
    "index.js",
    "node_modules"
  ],
  "dependencies": {
    "onoff": "^1.2.0"
  },
  "moziot": {
    "api": {
      "min": 1,
      "max": 2
    },
    "plugin": true,
    "exec": "{nodeLoader} {path}",
    "config": {
      "gpios": [
	{
	  "pin": 18,
	  "name": "led",
	  "direction": "out",
	  "value": 0
	}
      ]
    },
    "schema": {
      "type": "object",
      "properties": {
        "gpios": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "pin"
            ],
            "properties": {
              "pin": {
                "type": "integer",
		"minimum": 0
              },
              "name": {
                "type": "string"
              },
              "direction": {
                "type": "string",
                "enum": [
                  "in",
                  "out"
                ]
              },
              "value": {
                "type": "number",
		"enum": [
		  0,
		  1
		]
              },
	      "edge": {
		"type": "string",
		"enum": [
		  "none",
		  "rising",
		  "falling",
		  "both"
		]
	      },
	      "debounce": {
		"type": "number",
		"minimum": 0
	      }
            }
          }
        }
      }
    }
  }
}
```

The `moziot` fields are explained as follows:

- `api.min` - The minimum API version supported.
- `api.max` - The maximum API version supported.
- `plugin` - Whether or not this add-on is a plugin, meaning it runs in its own process. This should always be `true`.
- `exec` - The command to execute to start this add-on.
- `config` - (Optional) Generic configuration object. When the user changes config options, they will be stored in the gateway's settings database, and will persist across package upgrades.
- `schema` - (Optional) JSON Schema Validation object for the `config` object. This will be used to generate an add-on configuration UI.