High level overview of how the Adapter IPC works.

Currently, the IPC code relies on a package called [nanomsg](http://nanomsg.org/) which has bindings for a variety of languages.

# Files

A new adapter, called `plugin` was added. The contains the following files:

| Filename | Purpose |
| -------- | ------- |
| index.js | Loads plugin-server |
| ipc.js | Implements IpcSocket which is used by the Adapter and gateway comms |
| plugin-server.js | Implements the gateway side of adapter registration. |
| plugin-client.js | Implenents the adapter side of adapter registration. |
| adapter-proxy.js | Proxy object used on the gateway to represent and actual adapter in the adapter. |
| device-proxy.js | Proxy object used on the gateway to represent an actual device in the adapter.|
| property-proxy.js | Proxy object used on the gateway to represent an actual property in the adapter. |
| adapter-manager-proxy.js | Proxy object used on the adapter to represent the adapter manager in the gateway. |

And a test plugin which is a variation of the mock plugin is implemented in the test-plugin directory.

| Filename | Purpose |
| -------- | ------- |
| index.js | Part of an example adapter. |
| test-plugin.js | An example adapter. |
| load.js | App which instantiates a PluginClient and then loads the adapter. No changes to the actual adapter should be required. |

# Overview

The plugin server implements a listener using a [request-reply nanomsg protocol](http://nanomsg.org/v1.0.0/nn_reqrep.7.html). There is a single request 'registerAdapter' which is used to register a new adapter plugin. The gateway will the allocate some per-adapter resources using a [pair nanomsg protocol](http://nanomsg.org/v1.0.0/nn_pair.7.html). All further communications between the adapter and the gateway will occur on the this paired channel.

On the gateway side, the adapter/device/protocol proxies will translate API calls into IPC calls, and convert IPC replies/notifications appropriately.

On the adapter size, the adapter manager proxy performs similar functionality.

# ToDo

* a mechanism to detect that the gateway side has crashed or otherwise "gone away" and reappeared needs to be implemented so that the adapter can re-register. I think that the gateway side can detect the named-pipe files to detect which adapters may have been previously started. Alternately, it may be able to use the repreq channel, or may need to implement one of the broadcast mechanisms that nanomsg provides (or perhaps change the reqrep to be a bus).
* a mechanism to start a plugin
* a mechanism to restart a plugin when it crashes or otherwise "goes away".
* add timeouts to message send-get-reply so that it can gracefully recover.

# How to run the test-plugin

1. Start the gateway with the plugin adapter enabled.
2. Start the test-plugin using the command `node adapter-loader.js test_plugin` from the directory containing the adapter-loader.js file.

# IPC Messages

## Adapter Registration

The following message and reply are used when an adapter registers itself with the gateway. The gateway opens a `rep` channel using `ipc:///tmp/gateway.adapterManager`, and the adapter should use a `req` channel.

### registerAdapter

Sent from the adapter to the gateway to register the adapter with the gateway.
```
{
  messageType: 'registerPlugin',
  data: {
    pluginId: 'pluginId-string',
  },
}
```
The `pluginId` will be unique to the plugin, and should match the key used in the config for `adapters`. (i.e. 'zwave').

### registerPluginReply
Reply sent from the gateway back to the adapter.
```
{
  messageType: 'registerPluginReply',
  data: {
    pluginId: 'pluginId-string',
    ipcBaseAddr: 'gateway.plugin.xxx',
  },
}
```
The `xxx` in the `ipcBaseAddr` will be replaced by the `pluginId`. This is the base portion name of the per-adapter `pair` IPC channel that the gateway allocates for the adapter. All further communications between the gateway and the adapter should take place on this channel. The full ipc address will be  combined remaining portion will be determined based on the protocol specified via the config ipc.protocol setting. 

## Adapter Messages

### addAdapter
Sent from the adapter to the gateway to indicate that the adapter is ready.
```
{
  messageType: 'addAdapter',
  data: {
    adapterId: 'adapterId-string',
    name: 'name-of-the-adapter',
  },
}
```

### handleDeviceAdded
Sent from the adapter to the gateway to indicate that a new device has been added. Note that these may be sent before the addAdapter message is sent.
```
{
  messageType: 'handleDeviceAdded',
  data: device.asDict(),
}
```
The device.asDict() will return a dictionary representation of the device. See the asDict() method in the Device class for details.

### handleDeviceRemoved
Sent from the adapter to the gateway to indicate that a previously added device has been removed.
```
{
  messageType: 'handleDeviceRemoved',
  data: {
    id: deviceId,
  },
}
```
The deviceId should match the id field sent in the handleDeviceAdded notification.

### setProperty
Sent from the gateway to the adapter to set the value of a property contained on the indicated device.
```
{
  messageType: 'setProperty',
  data: {
    deviceId: 'device-id',
    propertyName: 'name-of-property',
    propertyValue: new-value,
  },
}
```

### propertyChanged
Sent from the adapter to the gateway anytime a change in property value is detected. A propertyChanged notification will always be sent in response to a setProperty, regardless of whether the value actually changes or not.
```
{
  messageType: 'propertyChanged',
  data: {
    deviceId: 'device-id',
    propertyName: 'name-of-property',
    propertyValue: updated-value,
  },
}
```

### startPairing
Sent from the gateway to the adapter to put the adapter into pairing mode.
```
{
  messageType: 'startPairing',
  data: {
    timeout: timeoutInSeconds,
  },
}
```

### cancelPairing
Sent from the gateway to the adapter to cancel pairing mode.
```
{
  messageType: 'cancelPairing',
}
```

### removeThing
Sent from the gateway to the adapter to initiate device removal.
```
{
  messageType: 'removeThing',
  data: {
    deviceId: 'device-id-to-remove',
  },
}
```

### cancelRemoveThing
Sent from the gateway to the adapter to cancel a previously initiated device removal.
```
{
  messageType: 'cancelRemoveThing',
  data: {
    deviceId: 'device-id-to-remove',
  },
}
```
