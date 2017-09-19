The gateway has a debug controller which can be useful if you're developing adapters. By default, the debug controller is disabled. 

## Turn on the debug controller

To turn on the debug controller, run the gateway using the -d option.

```
npm start -- -d
```
If you're running the gateway on a Raspberry Pi, then the gateway is normally started automatically using systemd, so use this command to stop the gateway first:
```
sudo systemctl stop mozilla-iot-gateway
```
prior to starting the gateway with the debug controller enabled.

## Verify debug controller is running

You can verify that the debug controller has been enabled by issuing a command like this:
```
curl -v -H "Accept: application/json" https://localhost:4443/debug/devices/ -k | python -m json.tool | less
```
Using python's json.tool provides pretty formatting.

You can also use a tool like [Postman](https://www.getpostman.com/) which will allow you to save requests. If you're using a self-signed certificate, make sure you turn off the "SSL certificate validation" setting (found in  File->Settings->General)

## Debug Controller URLs

| URL | Op | Description |
| --- | -- | ----------- |
| /debug/devices | GET | Returns all of the devices that the adapter manager knows about. The data returned may contain lots of internal information not exposed using the Web Thing representation. |
| /debug/device/**deviceId** | GET | Returns the information for a single device. |
| /debug/device/**deviceId**/**propertyName** | GET | Returns the current value of **propertyName**. |
| /debug/device/**deviceId**/**propertyName** | PUT | Sets the value of **propertyName**. |
| /debug/things | GET | Returns all of the things that the adapter manager knows about, and uses the Web Thing representation. |
| /debug/things/**thingId** | GET | Returns the Web Thing representation for a single thing. |
| /debug/things/**thingId**/**propertyName** | GET | Returns the current value of **propertyName**. |
| /debug/things/**thingId**/**propertyName** | PUT | Sets the value of **propertyName**. |
| /debug/addNewThing | GET | Initiates pairing |
| /debug/cancelAddNewThing | GET | Cancels pairing |
| /debug/removeThing/**thingId** | GET | Initiates the removal of a specific thing. |
| /debug/cancelRemoveThig | GET | Cancels the removal of a thing. |
