High level overview of how the gateway's add-on IPC works.

# Files

In the gateway, a plugin interface exists in `src/plugin/`. It contains the following files:

| Filename | Purpose |
| -------- | ------- |
| plugin-server.js | Implements the gateway side of add-on registration. |
| plugin.js | Wrapper around a plugin process. Instantiated by `plugin-server.js`. |
| adapter-proxy.js | Proxy object used by the gateway to represent an actual adapter in the add-on. |
| device-proxy.js | Proxy object used by the gateway to represent an actual device in the add-on. |
| property-proxy.js | Proxy object used by the gateway to represent an actual property in the add-on. |
| notifier-proxy.js | Proxy object used by the gateway to represent an actual notifier in the add-on. |
| outlet-proxy.js | Proxy object used by the gateway to represent an actual outlet in the add-on. |
| api-handler-proxy.js | Proxy object used by the gateway to represent an actual API handler in the add-on. |

The client side, as well as the actual IPC implementation, reside in the gateway-addon libraries. For the gateway itself, as well as Node.js add-ons, this is [gateway-addon-node](https://github.com/mozilla-iot/gateway-addon-node/tree/master/lib). The relevant IPC files here are the following:

| Filename | Purpose |
| -------- | ------- |
| ipc.js | Implements IpcSocket which is used by the add-on and gateway comms. |
| plugin-client.js | Implements the add-on side of add-on registration. |
| addon-manager-proxy.js | Proxy object used by the add-on to represent the add-on manager in the gateway. |

# Overview

Starting with WebThings Gateway version 0.12, the plugin server opens a single WebSocket on localhost:9500. There is an initial Plugin Register Request, which is used to register a new plugin. The gateway will then allocate some per-adapter resources internally. All further communications between the add-on and the gateway will occur through the same WebSocket, but each message will have an associated `pluginId` field.

All messages are encoded as JSON. A schema containing all valid messages, as well as their descriptions, lives [here](https://github.com/mozilla-iot/gateway-addon-ipc-schema).

On the gateway side, the adapter/device/property/etc. proxies will translate API calls into IPC calls, and convert IPC replies/notifications appropriately.

On the add-on side, the add-on manager proxy performs similar functionality.

# IPC Messages

## Plugin Registration

The following message and reply are used when an add-on registers itself with the gateway. The gateway opens a WebSocket server on localhost:9500, which the add-on will connect to.

### Plugin Register Request

Sent from the add-on to the gateway to register the add-on with the gateway.
```json
{
  "messageType": 0,
  "data": {
    "pluginId": "pluginId-string"
  }
}
```
The `pluginId` will be unique to the plugin, and should match the `id` key used in the add-on's `manifest.json` (e.g. 'zwave-adapter').

### Plugin Register Response

Reply sent from the gateway back to the add-on.
```json
{
  "messageType": 1,
  "data": {
    "pluginId": "pluginId-string",
    "gatewayVersion": "0.12.0",
    "userProfile": {
      "addonsDir": "/home/pi/.mozilla-iot/addons",
      "baseDir": "/home/pi/.mozilla-iot",
      "configDir": "/home/pi/.mozilla-iot/config",
      "dataDir": "/home/pi/.mozilla-iot/data",
      "mediaDir": "/home/pi/.mozilla-iot/media",
      "logDir": "/home/pi/.mozilla-iot/log",
      "gatewayDir": "/home/pi/mozilla-iot/gateway",
    },
    "preferences": {
      "language": "en-US",
      "units": {
        "temperature": "degree celsius"
      }
    }
  }
}
```

## Resources

* All other messages, as well as the plugin registration messages above, are defined in the [mozilla-iot/gateway-addon-ipc-schema](https://github.com/mozilla-iot/gateway-addon-ipc-schema) repo.
* The Node.js bindings, used by both the gateway itself and Javascript add-ons, can be found in the [mozilla-iot/gateway-addon-node](https://github.com/mozilla-iot/gateway-addon-node) repo.
* Python bindings can be found in the [mozilla-iot/gateway-addon-python](https://github.com/mozilla-iot/gateway-addon-python) repo.

## Further Thoughts

Given that the IPC layer is based on WebSockets, it should be fairly straightforward to implement add-ons in essentially any language. The easiest way to start would be to take a look at the IPC and AddonManagerProxy implementations in either of the two existing binding libraries and go from there.