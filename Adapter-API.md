# Adapter Code

The documentation below is focused on Node.js adapters, but other languages are supported as well. The package will be loaded into the gateway dynamically, provided that its API version is supported and it provides a proper manifest.

The package should have one exported function, which will be executed on startup. It will be called as such:

```
loadMyAdapter(adapterManagerInstance, manifestObject, errorCallback);
```

The `manifestObject` will be an object containing the data from the manifest, overlaid with the saved configuration (if present).

The load function should call `errorCallback(manifestObject.id, errorStr)` if it could not be loaded.

The load function should create an object that is a subclass of `Adapter`, and can override the following class methods:

- `constructor(adapterManagerInstance, id, packageId)` - Initialize the object with the instance of the adapter manager, the adapter's individual ID, and the adapter's package ID.
- `dump()` - Dump the state of the adapter to the log.
- `getId()` - Get the ID of this adapter.
- `getPackageId()` - Get the package ID of this adapter.
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
    - `properties` - The properties of the device, as a `Map`.
    - `actions` - The actions associated with the device, as a `Map`.
- `asThing()` - Return the device state as a `thing` object, with at least the following members:
    - `id` - The device ID.
    - `name` - The name of the device.
    - `type` - The type of the device.
    - `properties` - The properties of the device, as returned by `getPropertyDescriptions()`.
    - `description` - (Optional) The description of the device.
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
- `notifyPropertyChanged(property)` - Notify the adapter manager that the given property has changed. This should emit a `Constants.PROPERTY_CHANGED` event, containing the changed property.
- `setDescription(description)` - Set the description of the device.
- `setName(name)` - Set the name of the device.
- `setProperty(propertyName, value)` - Set the given property of the device.

To define new device properties, the package can optionally subclass `Property`, which provides the following methods:

- `constructor(device, name, type)` - Initialize the object with the given device, property name, and property type.
- `asDict()` - Return the property state as an object, with at least the following members:
    - `name` - The name of the property.
    - `type` - The type of the property.
    - `value` - The value of the property.
    - `description` - (Optional) The description of the property.
- `asPropertyDescription()` - Return the property description as an object, with at least the following members:
    - `type` - The type of the property.
    - `description` - (Optional) The description of the property.
    - `unit` - (Optional) The unit of the property.
- `setCachedValue(value)` - Sets `this.value` and makes adjustments to ensure that the value is consistent with the type. The adjusted value should be returned.
- `getValue()` - Return a `Promise` that will resolve to the property's value.
- `setValue(value)` - Return a `Promise` that will resolve to the set value, after adjustments by `setCachedValue(value)`.

## Available Imports for Adapters

The classes named above can be imported as such:

```
const Adapter = require('../adapter');
const Device = require('../device');
const Property = require('../property');
```

See issue for discussion https://github.com/moziot/gateway/issues/1