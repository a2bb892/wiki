Eventually we'd like it to be possible to write adapters (e.g. ZigBee adapter, Z-Wave adapter) in different programming languages, have them run in their own process, and talk to them via some kind of IPC mechanism.

But for our initial NodeJS prototype we’ve discussed having some kind of uniform JavaScript interface for the web app to talk to adapters.

Based on the [Web Thing REST API](https://moziot.github.io/wot/) which will be a layer above this JavaScript API, my first thoughts on the what basic underlying features of the adapter layer might be are:

**Adapter**

- **discoverDevices()** // List available or connected devices so we can compare with list of devices already added
- **connectDevice()** // Pair with the device and/or get a connected device's details to add it to the gateway database
- **disconnectDevice()** // Unpair device if such a mechanism exists for this adapter

**Thing**
- **getProperty()** // Get a property value by its name
- **setProperty()** // Set a property value by its name
- **executeAction()** // What kinds of functions can we execute on  ZigBee/Z-Wave devices?
- **subscribe()**  // What kinds of events or changes we can listen or subscribe to from ZigBee/Z-Wave devices?

Does this make sense? What have I missed?