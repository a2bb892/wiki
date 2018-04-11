By default, the gpio-adapter only has a single gpio pin enabled, pin 18.

To view the GPIO configuration, you can navigate to _Settings -> Add-ons_ in the UI, and click `Configure` on the `gpio-adapter` entry.

The gpio-adapter uses the node library [onoff](https://www.npmjs.com/package/onoff) to configure the pins, so you can refer to that documentation for full details.

* `"pin"` is the pin number.
* `"name"` is an arbitrary name that you assign to the pin. This will show up in the UI as the default name.
* `"direction"` may be `"in"` or `"out"` (currently `"high"` and `"low"` are not supported).
  * If `direction` is set to `in`, the following may also be configured:
    * `"edge"` which may be one of `"none"`, `"rising"`, `"falling"`, or `"both"`.
    * `"debounce"` which is a number indicating the number of milliseconds that the input should be debounced for. If no debounce is provided, then a default of 10 milliseconds will be used. Setting debounce to 0 will disable the debounce logic.
  * If `direction` is set to `out`, the following may also be configured:
    * `"value"` which is the initial value

So for example, the following configuration would have pin 18 as an output and pin 23 as an input:
```
{
  "gpios": [
    {
      "pin": 18,
      "name": "led",
      "direction": "out",
      "value": 0
    },
    {
      "pin": 23,
      "name": "button",
      "direction": "in",
      "edge": "both",
      "debounce": 10
    }
  ]
}
```