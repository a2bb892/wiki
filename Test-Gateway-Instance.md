We host a public instance of the [Things Gateway](https://github.com/mozilla-iot/gateway) in the cloud for interoperability testing.

## Getting Started

Navigate to https://w3c-interop.mozilla-iot.org

![Login](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/login.png)

Enter username and password:
* **Email:** iot@mozilla.com
* **Password:** AreWeInter-opYet?

You will be presented with an empty things screen.

![Empty Things Screen](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/empty-things-screen.png)

Click the "+" button to add some virtual things (if you want to use real devices you'll need to [download](https://iot.mozilla.org/gateway/) and run the gateway yourself).

**Note:** The list of devices will be cleared every 24 hours.

![Add Things](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/add-things.png)

You will see a list of virtual things on the things screen.

![Virtual Things](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/virtual-things.png)

You can turn devices on and off by clicking on them. Click of the detail icon (looks like a splat) to view the detail view for a thing.

![Virtual Things](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/dimmable-color-light.png)

You can view and modify properties from the thing detail view.

## Inspecting API Requests

Using your browser's developer tools, you can inspect the [Web Thing API](https://iot.mozilla.org/wot/#web-thing-rest-api) requests being sent to the server.

### Get Thing

When you navigate to a thing detail view, the device's [Thing Description](https://iot.mozilla.org/wot/#web-thing-description) will be requested from the server to generate its user interface.

![Get Thing](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/get-thing.png)

### Get Property

The values of the thing's properties are fetched with an HTTP GET request.

![Get Property](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/get-property.png)

### Set Property

When you set a property in the UI, it is set with an HTTP PUT request.

![Set Property](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/set-property.png)

### Request Action

When you request an action in the UI, it is requested with an HTTP POST request.

![Request Action](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/request-action.png)

### Open WebSocket

When viewing a thing's detail, a WebSocket is opened on the thing to listen for messages over the [Web Thing WebSocket API](https://iot.mozilla.org/wot/#web-thing-websocket-api) (e.g. property status updates, action status updates and events).


![Open WebSocket](https://raw.githubusercontent.com/mozilla-iot/wiki/master/images/open-websocket.png)








