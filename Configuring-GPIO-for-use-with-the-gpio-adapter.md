By default, the gpio-adapter only has a single gpio pin enabled.

To view the GPIO configuration you need to login to the gateway using one of the methods described on the [Logging into the Raspberry Pi](https://github.com/mozilla-iot/wiki/wiki/Logging-into-the-Raspberry-Pi) page.

cd into the mozilla-iot/gateway/tools directory. You should see a file called config-editor.py. If you run:
`./config-editor.py gpio-adapter` you should see some output like the following:
```
pi@gateway:~/mozilla-iot/gateway/tools$ ./config-editor.py gpio-adapter
{
  "pins": {
    "18": {
      "direction": "out",
      "name": "led",
      "value": 0
    }
  }
}
```
If you run `./config-editor.py -e gpio-adapter` then it will launch the nano editor and you can edit the configuration. I recommand that you enter one pin at a time since any mistakes in the JSON will cause your changes to be tossed.

The gpio-adapter uses the node library [onoff](https://www.npmjs.com/package/onoff) to configure the pins, so you can refer to that documentation for full details.

`"name"` is an arbitrary name that you assign to the pin. This will show up in the UI as the default name.

`"direction"` may be `"in"` or `"out"` (currently `"high"` and `"low"` are not supported).

If the pin is configured as an input, then it may have an attribute called `"edge"` which may be one of `"none"`, `"rising"`, `"falling"`, or `"both"`.

Input pins may also have an attribute called "debounce" which is a number indicating the number of milliseconds that the input should be debounced for. If no debounce is provided, then a default of 10 milliseconds will be used. Setting debounce to 0 will disable the debounce logic.

If the pin is configured as an output, then you can provide an initial value by using the `"value"` attribute.

So for example, the following configuration would have pin 18 as an output and pin 23 as an input:
```
{
  "pins": {
    "18": {
      "direction": "out",
      "name": "led",
      "value": 0
    },
    "23": {
      "direction": "in",
      "name": "button",
      "edge": "both",
      "debounce": 10
    }
  }
}
```