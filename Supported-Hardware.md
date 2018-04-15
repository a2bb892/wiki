With Project Things, Mozilla aims to create interoperability and user choice on the Internet of Things. Mozilla does not endorse any particular smart home product or brand, but here are the devices and protocols which have been tested  so far with the [Things Gateway](https://iot.mozilla.org/gateway).

## Gateway
### Raspbian
* Raspberry Pi 1 Model B+
* Raspberry Pi 2 Model B
* [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/) (recommended)
* Raspberry Pi 3 Model B+
* Raspberry Pi Zero W

### OpenWrt
* (Experimental OpenWrt package coming soon)

***

## Adapters
### Zigbee
* [Digi XStick](https://www.digi.com/products/xbee-rf-solutions/boxed-rf-modems-adapters/xstick) (ZB mesh version)

### Z-Wave
* [Sigma Designs UZB Stick](http://www.vesternet.com/z-wave-sigma-designs-usb-controller)
* [Aeotec Z-Stick](http://aeotec.com/z-wave-usb-stick) (Gen5)
* Any [OpenZWave compatible dongle](https://github.com/OpenZWave/open-zwave/wiki/Controller-Compatibility-List) should work, but have not all been tested

**Note:** Additional adapters can be supported via add-ons, which are being added to all the time.

***

## Devices
### Zigbee
* [SmartThings Power Outlet](http://www.samsung.com/uk/smartthings/sensors-plug-f-app-uk-v2/) (UK & US)
* [GE ZB4101 - On/Off Light and Small Appliance Module](https://byjasco.com/products/ge-zigbee-plug-smart-switch)
* [GE ZB3101 - Dimmer Lamp Module](https://byjasco.com/products/ge-zigbee-plug-smart-dimmer)
* [Sylvania SMART+ Smart Plug 729222-A](https://consumer.sylvania.com/our-products/smart/smart-connected-lighting/index.jsp)
* Cree Connected Bulbs
* IKEA Connected Bulbs
* Philips Hue Bulbs (requires a remote to factory reset)
* Many other Zigbee smart plugs, sensors, switches and bulbs using the Home Automation profile should also work. Please add devices you have tested to this wiki page.
### Z-Wave
* [Aeotec Smart Switch 6](https://aeotec.com/z-wave-plug-in-switch)
* [Aeotec Smart Dimmer 6](https://aeotec.com/z-wave-plug-in-dimmer)
* [GoControl Z-Wave Bulb](http://www.gocontrol.com/detail.php?productId=7)
* [Leviton VRPA1 - Plug-in Outlet Module](http://www.leviton.com/en/products/dzpa1-2bw) The VRPA1 apepars to have been suprceded by the DZPA1.
* [GE ZW4103 - Plug-in Smart Switch](https://products.z-wavealliance.org/products/1435)
* [Inovelli NZW37 2 Channel Dual Smart Plug](https://inovelli.com/shop/smart-plugs/2-channel-smart-plug/)  Only channel 1 is detected
* [Inovelli OM71V 1 Channel Smart Plug - Outdoor Rated](https://inovelli.com/shop/outdoor-smart-plugs/outdoor-z-wave-plug-in-module-1-channel/) 
* [Devolo Smartmetering Plug](https://www.devolo.com/en/Products/devolo-Home-Control-Smart-Metering-Plug)
* [Neo Coolcam smart power Plug](http://www.szneo.com/en/products/index.php?id=41)
* [Fibaro Wall Plug 2 FGWPF-102-ZW5](https://www.fibaro.com/en/products/wall-plug/)
* Many other Z-wave smart plugs, sensors, switches and bulbs should also work. Please add devices you have tested to this wiki page.
### Wi-Fi
* TP-Link
  * [bulbs](http://www.tp-link.com/us/home-networking/smart-home/smart-bulbs)
  * [switches](http://www.tp-link.com/us/home-networking/smart-home/smart-switches)
  * [plugs](http://www.tp-link.com/us/home-networking/smart-home/smart-plugs)
* Philips Hue
  * [bulbs](https://www2.meethue.com/en-us/products#filters=STARTER_KITS_SU%2CBULBS_SU%2CLIGHTSTRIPS_SU%2CLAMPS_SU%2CCONTROLS_SU&sliders=&support=&price=&priceBoxes=&page=&layout=12.subcategory.p-grid-icon) -- using Zigbee to Wi-Fi bridge
### HomeKit
**Note**: Devices typically need to be unpaired from Android/iOS before using them with the gateway.

* Smart plugs (represented as `onOffSwitch`)
  * [iDevices Switch](https://store.idevicesinc.com/idevices-switch/)
  * [Koogeek P1](https://www.koogeek.com/p-p1.html)
  * All other WiFi-based HomeKit smart plugs should also work
* Bridges
  * [Homebridge](https://github.com/nfarina/homebridge)
  * [Philips Hue Bridge v2](https://www2.meethue.com/en-us/p/hue-bridge/046677458478)
  * Other HomeKit bridges should also work## 

### GPIO
* Raspberry Pi GPIO