There is a command line tool that I use for exploring new devices with the zigbee and zwave adapters.

You can find it here: https://github.com/mozilla-iot/cli

## Start the gateway in debug mode

In order to get the best information, the gateway needs to run in debug mode, which can be achieved by adding the -d option to the gateway startup. You can do this by editing the run-app.sh script found in `/home/pi/mozilla-iot/gateway`. Search for the line that looks like:
```
  npm run "${start_task}" -- $args
```
and add a -d so that it looks like this:
```
  npm run "${start_task}" -- -d $args
```

You can then restart the gateway service using:
```
sudo systemctl restart mozilla-iot-gateway
```

## Start the command line tool

To run, use the c.py script. You should then be presented with a prompt like:
```
cli>
```
You can use the help command to discover available commands and `help command` to get more information about a particular command.

The first command you'll want to use is the gateway command. You should use something like:
```
cli> gateway http://localhost:8080
```
or
```
cli> gateway https://localhost:4443
```
or
```
cli> gateway https://my-gateway-name.mozilla-iot.org
```

You can then see the devices by using the devices command (the devices and device commands require that the gateway be running in debug mode).

You can use the `device` command along with one of the names presented by the devices command to select that device as a target.

The `info` command is the command that I use to get internal information about a particular device.

Use Control-D to exit from the device and go back up to the gateway level if you want to select another device.

The `info` command can also take a filename, in which case the output of the info command will be saved into that file (in JSON format). This output is what is used for the zwave-adapter classifier tests. You can find some examples here: https://github.com/mozilla-iot/zwave-adapter/tree/master/test/classifier

## Sample Session

A sample session looks something like this:
```
cli> gateway http://localhost:8080
cli http://localhost:8080> 
cli http://localhost:8080> devices
zwave-d7e613b4-11 zwave-d7e613b4-12 zwave-d7e613b4-4 
cli http://localhost:8080> device zwave-d7e613b4-4
cli http://localhost:8080 zwave-d7e613b4-4> info
{
  "baseHref": null,
  "pin": {
    "required": false,
    "pattern": null
  },
  "credentialsRequired": false,
  "lastStatus": "ready",
  "zwInfo": {
    "location": "",
    "nodeId": 4,
    "manufacturer": "AEON Labs",
    "manufacturerId": "0x0086",
    "product": "ZW100 MultiSensor 6",
    "productId": "0x0064",
    "productType": "0x0102",
    "type": "Home Security Sensor",
    "genericType": 33,
    "basicType": 4,
    "specificType": 1
  },
  "zwClasses": [
    48,
    49,
    94,
    112,
    113,
    114,
    115,
    128,
    132,
    134
  ],
  "zwValues": {
    "4-48-1-0": {
      "value_id": "4-48-1-0",
      "node_id": 4,
      "class_id": 48,
      "type": "bool",
      "genre": "user",
      "instance": 1,
      "index": 0,
      "label": "Sensor",
      "units": "",
      "help": "Binary Sensor State",
      "read_only": true,
      "write_only": false,
      "min": 0,
      "max": 0,
      "is_polled": false,
      "value": true
    },
...snipped output...
    "batteryLevel": {
      "name": "batteryLevel",
      "value": 100,
      "visible": true,
      "title": "Battery",
      "type": "number",
      "@type": "LevelProperty",
      "unit": "percent",
      "minimum": 0,
      "maximum": 100,
      "readOnly": true,
      "valueId": "4-128-1-0"
    }
  },
  "actions": {},
  "events": {},
  "links": []
}
cli http://localhost:8080 zwave-d7e613b4-4>
```

Some commands may require you login to the gateway. In this case you'll be prompted for your email and password. The JWT (JSON Web Token) will be stored in the ~/.moziot-cli.json file, so you don't need to enter it again.