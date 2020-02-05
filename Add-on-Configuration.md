The documentation below is focused on Node.js adapters, but other languages are supported as well.
The add-on can publish a configuration page on the gateway URL `/settings/addons/config/youraddon`, provided that it provides a JSON-schema in its manifest.

## Basic configuration

If the add-on provides configurable values, `manifest.json` should have an `options` field.

**Example 1:** JSON-schema for simple configuration.
```json
{
  "options": {
    "schema": {
      "type": "object",
      "properties": {
        "foo": {
          "type": "string"
        },
        "bar": {
          "type": "string"
        }
      }
    }
  }
}
```

Configuration page for Example 1:

![image](https://user-images.githubusercontent.com/20137651/39053305-a98f91cc-44e9-11e8-946e-ef292a6b5c02.png)

When the _Apply_ button is pushed, the add-on is reloaded and can retrieve the user input with the `Database` class.

```js
const {Database} = require('gateway-addon');
const manifest = require('./manifest.json');

loadMyAdapter(addonManager, _, errorCallback) {
  const db = new Database(manifest.id);
  db.open().then(() => {
    return db.loadConfig();
  }).then((config) => {
    // config contains values which user input.
  }).catch(console.error);
}
```

## Configuration with default values

If the add-on should have default values which are configurable in order to work without configuration, `manifest.json` should have a `default` field inside `options`.

**Example 2:** JSON-schema and default values for simple configuration.

```json
{
  "options": {
    "default": {
      "foo": "default value foo",
      "bar": "default value bar"
    },
    "schema": {
      "type": "object",
      "properties": {
        "foo": {
          "type": "string"
        },
        "bar": {
          "type": "string"
        }
      }
    }
  }
}
```

Configuration page for Example 2:

![image](https://user-images.githubusercontent.com/20137651/39053906-30f26a9e-44eb-11e8-9b7a-7ea3a2a448f8.png)

## Configuration with description

The add-on can also provide descriptions about configurable values.

**Example 3:** JSON-schema for configuration with description.

```json
{
  "options": {
    "schema": {
      "type": "object",
      "description": "this is description",
      "properties": {
        "foo": {
          "type": "string",
          "description": "this is description for foo"
        },
        "bar": {
          "type": "string",
          "description": "this is description for bar"
        }
      }
    }
  }
}
```

Configuration page for Example 3:

![image](https://user-images.githubusercontent.com/20137651/39055236-a0db39e6-44ee-11e8-9253-b16ac4f5e586.png)

## Configuration which have variable length values

If the add-on provides configurable values which are variable length, JSON-schema should have array field.

**Example 4:** JSON-schema for configuration which have variable length values.

```json
{
  "options": {
    "schema": {
      "type": "array",
      "title": "addable field",
      "items": {
        "type": "string"
      }
    }
  }
}
```

Configuration page for Example 4:

![image](https://user-images.githubusercontent.com/20137651/39057162-8d75f5c6-44f3-11e8-9544-e85317698c29.png)

`+` button clicked:

![image](https://user-images.githubusercontent.com/20137651/39057937-7c1a1e9a-44f5-11e8-9f5e-31523815ff11.png)

## Configuration with dynamic form.

If the add-on provides a dynamic form which is related with some configurable values, JSON-schema should have `oneOf` field and `dependencies` field.

**Example 5:** JSON-schema for configuration with dynamic form.

```json
{
  "options": {
    "schema": {
      "title": "Device",
      "type": "object",
      "properties": {
        "deviceType": {
          "type": "string",
          "enum": [
            "light",
            "dimmingLight",
            "colorLight"
          ],
          "default": "light"
        }
      },
      "required": [
        "deviceType"
      ],
      "dependencies": {
        "deviceType": {
          "oneOf": [
            {
              "properties": {
                "deviceType": {
                  "enum": [
                    "light"
                  ]
                }
              }
            },
            {
              "properties": {
                "deviceType": {
                  "enum": [
                    "dimmingLight"
                  ]
                },
                "recommendBrightness": {
                  "type": "number"
                }
              },
              "required": [
                "recommendBrightness"
              ]
            },
            {
              "properties": {
                "deviceType": {
                  "enum": [
                    "colorLight"
                  ]
                },
                "recommendColor": {
                  "type": "string"
                }
              },
              "required": [
                "recommendColor"
              ]
            }
          ]
        }
      }
    }
  }
}
```

Configuration page for Example 5:

![image](https://user-images.githubusercontent.com/20137651/39060778-436b02d2-44fd-11e8-9c90-bd70f241911a.png)

Change `deviceType` to `dimmingLight`:

![image](https://user-images.githubusercontent.com/20137651/39060853-70004ac8-44fd-11e8-9eff-700e6d6e0d29.png)

Change `deviceType` to `colorLight`:

![image](https://user-images.githubusercontent.com/20137651/39060889-8d84599a-44fd-11e8-9234-1e225392a0b3.png)

## Other use cases

If you need more information about generating configuration with JSON-schema, see [here](https://github.com/mozilla-services/react-jsonschema-form).
The gateway has jsonschema-form which is ported without `uiSchema` and `customForm`.

### Provide device-specific information to user.

The use case is based on [this question](https://discourse.mozilla.org/t/add-on-specific-html-pages-to-configure-things-default-settings/27816/3).

If the add-on provides some device-specific information in order to help a user input configuration values,"e.g. How many devices should be configured?", the add-on can add information to the config.

**Example 6:** JSON-schema for helping a user input values

```json
{
  "options": {
    "schema": {
      "type": "object",
      "properties": {
        "WiFiAccessPoint": {
          "type": "object",
          "properties": {
            "SSID": {
              "type": "string"
            },
            "password": {
              "type": "string"
            }
          },
          "required": [
            "SSID",
            "password"
          ]
        },
        "discoveredDevices": {
          "type": "array",
          "description": "Dont add or delete discoveredDevices.",
          "items":{
            "type": "object",
            "properties": {
              "name":{
                "type": "string"
              },
              "pin1": {
                "type": "string"
              },
              "pin2": {
                "type": "string"
              }
            }
          }
        }
      }
    }
  }
}
```

The add-on code for helping a user input values:

```js
const {Adapter, Database} = require('gateway-addon');

class SettingsTestAdapter extends Adapter {
  constructor(addonManager, packageName) {
    super(addonManager, packageName, packageName);
    addonManager.addAdapter(this);

    // if discovered dvices, add to config.
    const discoveredDevices = [
      {name: 'device1'},
      {name: 'device2'},
      {name: 'device3'},
    ];

    const db = new Database(packageName);
    db.open().then(async () => {
      const config = await db.loadConfig();
      config.discoveredDevices = discoveredDevices;
      db.saveConfig(config);
    });
  }
}
```

Configuration page for Example 6:

First step, the user input WiFi configuration for connecting an router and devices.

![image](https://user-images.githubusercontent.com/20137651/39063736-fbad0f26-4506-11e8-9e49-a589b3fc83db.png)

Second step, the user input the device configuration for using the device.

![image](https://user-images.githubusercontent.com/20137651/39064028-e809fac8-4507-11e8-9c8c-108888b6e985.png)