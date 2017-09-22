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
2. Start the test-plugin using the command `node load.js`