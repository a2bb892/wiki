High level overview of how the Adapter IPC works.

Currently, the IPC code relies on a package called [nanomsg](http://nanomsg.org/) which has bindings for a variety of languages.

## Why nanomsg?

While searching for an IPC library to use, ZeroMQ seemed to come up as a logical choice. However, while investigating issues that ZeroMQ might have, I came across another library called nanomsg (written by the same author as ZeroMQ), which attempted to address those issues. This [article](http://bravenewgeek.com/a-look-at-nanomsg-and-scalability-protocols/) dicusses some of those issues.

Here's a list of things which also made nanomsg favorable (in my mind).
- nanomsg is licensed under an [MIT/X11](https://github.com/nanomsg/nanomsg/blob/master/COPYING) license, which is compatible with the MPL license used by the gateway project.
- nanomsg is written in C, which simplifies the process of creating language bindings.
- nanomsg has support for [numerous languages](http://nanomsg.org/documentation.html)
- nanomsg is cross platform, and supports Linux, OSX, and Windows.
- nanomsg has no further dependencies.
- nanomsg is relatively small.
- nanomsg is a pure comms layer and doesn't try to impose any restrictions on the content of the messages.
- nanomsg is a message based IPC mechanism, versus an API based (i.e. RPC) mechanism, which seemed more suitable for sending JSON based messages.
- nanomsg was written by the same author who wrote ZeroMQ and attempts to overcome some of the shortcomings that ZeroMQ had. This [page](http://nanomsg.org/documentation-zeromq.html) describes some of the differences between nanomsg and ZeroMQ.

# Files

A new adapter, called `plugin` was added. The contains the following files:

| Filename | Purpose |
| -------- | ------- |
| index.js | Loads plugin-server |
| ipc.js | Implements IpcSocket which is used by the Adapter and gateway comms |
| plugin-server.js | Implements the gateway side of adapter registration. |
| plugin-client.js | Implenents the adapter side of adapter registration. |
| adapter-proxy.js | Proxy object used on the gateway to represent an actual adapter in the adapter. |
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

1. Start the gateway with the example plugin enabled (i.e. the config file should have an entry like this:
```
    example_plugin: {
      enabled: true,
      plugin: true,
      path: './adapters/example-plugin',
    },
```
2. Start the test-plugin using the command `node src/adapter-loader.js example_plugin` from the top level directory in the repository (i.e. the directory containing the `src` and `config` directories).

# IPC Messages

## Plugin Registration

The following message and reply are used when an adapter registers itself with the gateway. The gateway opens a `rep` channel using `ipc:///tmp/gateway.adapterManager`, and the adapter should use a `req` channel.

### registerPlugin

Sent from the plugin to the gateway to register the plugin with the gateway.
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
Reply sent from the gateway back to the plugin.
```
{
  messageType: 'registerPluginReply',
  data: {
    pluginId: 'pluginId-string',
    ipcBaseAddr: 'gateway.plugin.xxx',
  },
}
```
The `xxx` in the `ipcBaseAddr` will be replaced by the `pluginId`. This is the base portion name of the per-adapter `pair` IPC channel that the gateway allocates for the adapter. All further communications between the gateway and the adapter should take place on this channel. The full ipc address will be constructed based on the protocol specified via the config ipc.protocol setting.

| Protocol | Full IPC address |
| -------- | ---------------- |
| ipc | ipc:///tmp/{ipcBaseAddr} |
| inproc | inproc://{prefix}-{ipcBaseAddr} |

`inproc` is currently only used for testing, and `{prefix}` comes from the `AppInstance` class via the `get` method.

### unloadPlugin
Sent from the gateway to a plugin to tell the plugin that it should free up any resources its using, including closing the plugin side of the IPC channel.
```
{
  messageType: 'unloadPlugin',
  data: {
    pluginId: 'pluginId-string',
  },
}
```

### pluginUnloaded
Sent from the plugin to the gateway to indicate that it has completed unloaded. The plugin should keep the IPC channel open for a small amount of time after sending this message (about 500 milliseconds) to give the gateway a chance to see the message.
```
{
  messageType: 'pluginUnloaded',
  data: {
    pluginId: 'pluginId-string',
  },
}
```

## Adapter Messages

All of the adapter messages include a `pluginId` and `adapterId` fields. `pluginId` should match the `pluginId` sent in the `registerPlugin` request, and `adapterId` should match the `adapterId` sent in the `addAdapter` request.

### addAdapter
Sent from the adapter to the gateway to indicate that the adapter is ready.
```
{
  messageType: 'addAdapter',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
    name: 'name-of-the-adapter',
    packageName: 'name-of-the-package',
  },
}
```

### handleDeviceAdded
Sent from the adapter to the gateway to indicate that a new device has been added. Note that these may be sent before the addAdapter message is sent.
```
{
  messageType: 'handleDeviceAdded',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
    ... remaining fields from device.asDict() ...
  },
}
```
The device.asDict() will return a dictionary representation of the device. See the asDict() method in the Device class for details. The deviceId will be called simply `id`.

### handleDeviceRemoved
Sent from the adapter to the gateway to indicate that a previously added device has been removed.
```
{
  messageType: 'handleDeviceRemoved',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
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
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
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
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
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
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
    timeout: timeoutInSeconds,
  },
}
```

### cancelPairing
Sent from the gateway to the adapter to cancel pairing mode.
```
{
  messageType: 'cancelPairing',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
  },
}
```

### removeThing
Sent from the gateway to the adapter to initiate device removal.
```
{
  messageType: 'removeThing',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
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
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
    deviceId: 'device-id-to-remove',
  },
}
```

### unloadAdapter
Sent from the gateway to a plugin to tell a particular adapter to unload itself, and free up any resources that it may be using.
```
{
  messageType: 'unloadAdapter',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
  },
}
```

### adapterUnloaded
Sent from a plugin to the gateway to indicate that an adapter has completed unloading itself.
```
{
  messageType: 'adapterUnloaded',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
  },
}
```

### debugCmd
This is an optional message that can be used to send debug commands to an adapter. The debug_controller allows these types of commands to be sent.
```
{
  messageType: 'debugCmd',
  data: {
    pluginId: 'pluginId-string',
    adapterId: 'adapterId-string',
    deviceId: 'device-id-string',
    cmd: 'device-specific command string',
    params: 'command specific params'
  },
}
```


## Mock messages

There are some additional messages used for testing, which are only sent to the MockAdapter. See `src/adapters/mock/mock-adapter.js` for details.
