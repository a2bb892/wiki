# Adapter Code

The documentation below is focused on Node.js adapters, but other languages are supported as well. The package will be loaded into the gateway dynamically, provided that its API version is supported and it provides a proper manifest.

The package should have one exported function, which will be executed on startup. It will be called as such:

```javascript
loadMyAdapter(addonManagerInstance, manifestObject, errorCallback);
```

The `manifestObject` will be an object containing the data from the manifest, overlaid with the saved configuration (if present).

The load function should call `errorCallback(manifestObject.id, errorStr)` if it could not be loaded.

The load function should create an object that is a subclass of `Adapter`, and can override the following class methods:

- `constructor(addonManagerInstance, id, packageId)` - Initialize the object with the instance of the add-on manager, the adapter's individual ID, and the adapter's package ID.
- `dump()` - Dump the state of the adapter to the log.
- `getId()` - Get the ID of this adapter.
- `getPackageName()` - Get the package name of this adapter.
- `getDevice(id)` - Get a device managed by the adapter with the given ID.
- `getDevices()` - Return an object containing all of the managed devices.
- `getName()` - Get the name of the adapter.
- `isReady()` - Get the ready state of the adapter as a boolean.
- `asDict()` - Return the adapter state as an object, with at least the following members:
    - `id` - The ID of the adapter.
    - `name` - The name of the adapter.
    - `ready` - The ready state of the adapter.
- `handleDeviceAdded(device)` - Called to indicate that a device is now being managed by this adapter.
- `handleDeviceRemoved(device)` - Called to indicate that a device is no longer being managed by this adapter.
- `startPairing(timeoutSeconds)` - Start the pairing process, with the provided timeout.
- `cancelPairing()` - Cancel the pairing process.
- `removeThing(device)` - Unpair the given device with the adapter.
- `cancelRemoveThing(device)` - Cancel unpairing.
- `unload()` - Called when the adapter is being unloaded, i.e. the gateway is shutting down or the adapter has been disabled. Perform any necessary cleanup here. Returns a `Promise` that resolves when unloading is finished.

Additionally, the package can define a subclass of `Device` in order to override methods of the devices managed by the adapter, as such:

- `constructor(adapter, id)` - Initialize the object with the instance of the managing adapter and this device's ID.
- `asDict()` - Return the device state as an object, with at least the following members:
    - `id` - The device ID.
    - `name` - The name of the device.
    - `type` - The type of the device.
    - `description` - Description of the device
    - `properties` - The properties of the device, as a `Map`.
    - `actions` - The actions associated with the device, as a `Map`.
    - `events` - The events associated with the device, as a `Map`.
- `asThing()` - Return the device state as a `thing` object, with at least the following members:
    - `id` - The device ID.
    - `name` - The name of the device.
    - `type` - The type of the device.
    - `properties` - The properties of the device, as returned by `getPropertyDescriptions()`.
    - `description` - (Optional) The description of the device.
    - `actions` - (Optional) The actions of the device
    - `events` - (Optional) The events of the device
- `getId()` - Get the ID of this device.
- `getName()` - Get the name of this device.
- `getType()` - Get the type of this device.
- `getPropertyDescriptions()` - Return the device's properties as an object:
    ```
    {
      propertyName1: propertyDescription1,
      ...
    }
    ```
- `findProperty(propertyName)` - Get the device property with the given name.
- `getProperty(propertyName)` - Return a `Promise` which will resolve to the device property with the given name.
- `hasProperty(propertyName)` - Determine whether or not the device has the given property.
- `notifyPropertyChanged(property)` - Notify the add-on manager that the given property has changed. This should emit a `Constants.PROPERTY_CHANGED` event, containing the changed property.
- `actionNotify(action)` - Notify the add-on manager that an action's status has changed. This should emit a `Constants.ACTION_STATUS` event, containing the action.
- `eventNotify(event)` - Notify the add-on manager that an event occurred. This should emit a `Constants.EVENT` event, containing the event.
- `setDescription(description)` - Set the description of the device.
- `setName(name)` - Set the name of the device.
- `setProperty(propertyName, value)` - Set the given property of the device.
- `requestAction(actionId, actionName, input)` - Request that an action be performed. This should return a Promise which resolves when the action has been requested.
- `removeAction(actionId, actionName)` - Cancel and remove an existing action. This should return a Promise which resolves when the action has been cancelled and removed.
- `performAction(action)` - Do whatever is necessary to perform the given action. This should return a Promise which resolves when the action has been performed.
- `addAction(name, metadata)` - Add an action available to be taken to the device.
- `addEvent(name, metadata)` - Add an event available for subscription to the device.

To define new device properties, the package can optionally subclass `Property`, which provides the following methods:

- `constructor(device, name, type)` - Initialize the object with the given device, property name, and property type.
- `asDict()` - Return the property state as an object, with at least the following members:
    - `name` - The name of the property.
    - `value` - The value of the property.
    - `description` - (Optional) The description of the property.
    - `type` - (Optional) The type of the property value.
    - `unit` - (Optional) The unit of the property.
    - `maximum` - (Optional) Maximum value of the property.
    - `minimum` - (Optional) Minimum value of the property.
- `asPropertyDescription()` - Return the property description as an object, with at least the following members:
    - `description` - (Optional) The description of the property.
    - `type` - (Optional) The type of the property value.
    - `unit` - (Optional) The unit of the property.
    - `maximum` - (Optional) Maximum value of the property.
    - `minimum` - (Optional) Minimum value of the property.
- `isVisible()` - Return whether or not this property should be visible to the gateway.
- `setCachedValue(value)` - Sets `this.value` and makes adjustments to ensure that the value is consistent with the type. The adjusted value should be returned.
- `setCachedValueAndNotify(value)` - Calls `setCachedValue` and notifies the device if the value has changed. Returns true if the value has changed.
- `getValue()` - Return a `Promise` that will resolve to the property's value.
- `setValue(value)` - Return a `Promise` that will resolve to the set value, after adjustments by `setCachedValue(value)`.

## Available Imports for Node.js Adapters

The classes named above can be imported as such:

```javascript
const {
  Action,     // Action base class
  Adapter,    // Adapter base class
  Constants,  // Constants used throughout the package
  Database,   // Class for interacting with the gateway's settings database
  Deferred,   // Wrapper for a promise, primarily used internally
  Device,     // Device base class
  Event,      // Event base class
  Property,   // Property base class
  Utils,      // Utility functions
} = require('gateway-addon');
```

## Python Bindings

Python 2.X/3.X bindings are available as well, and provide an almost identical interface to the above. They can be imported through the `gateway_addon` package.

See here: https://github.com/mozilla-iot/gateway-addon-python