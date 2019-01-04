With Project Things, Mozilla aims to create interoperability and user choice on the Internet of Things. Mozilla does not endorse any particular smart home product or brand, but here are the devices and protocols which have been tested  so far with the [Things Gateway](https://iot.mozilla.org/gateway).

## Gateway

### Raspbian

* Raspberry Pi 1 Model B
* Raspberry Pi 1 Model B+
* Raspberry Pi 2 Model B
* [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) (RPi 3 models recommended)
* [Raspberry Pi 3 Model B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/)
* [Raspberry Pi 3 Model A+](https://www.raspberrypi.org/products/raspberry-pi-3-model-a-plus/)
* Raspberry Pi Zero W

### OpenWrt

* [Experimental OpenWrt package](https://github.com/openwrt/packages/tree/master/lang/node-mozilla-iot-gateway) (while not yet optimized for commercial use, we recommend Turris Omnia, Linksys WRT3200ACM, Espressobin, Raspberry Pi 3, or similar powerful processors and platforms with plenty of RAM and flash) 

### Debian

* Experimental [Linaro](https://www.linaro.org/) support for some [96boards](https://www.96boards.org/products/) hardware
  * DragonBoard 410c
  * HiKey960
  * Rock960
  * Ultra96

***

## Adapters

### Zigbee

* [Digi XStick](https://www.digi.com/products/xbee-rf-solutions/boxed-rf-modems-adapters/xstick) (ZB mesh version)
* [ConBee Zigbee USB stick](https://www.dresden-elektronik.de/funktechnik/solutions/wireless-light-control/gateways/conbee/?L=1&cHash=c9c902ccdb43164696acccf81b62b2bd)

### Z-Wave

* [Sigma Designs UZB Stick](http://www.vesternet.com/z-wave-sigma-designs-usb-controller)
* [Aeotec Z-Stick](http://aeotec.com/z-wave-usb-stick) (Gen5)
* Any [OpenZWave compatible dongle](https://github.com/OpenZWave/open-zwave/wiki/Controller-Compatibility-List) should work, but have not all been tested

**Note:** Additional adapters can be supported via add-ons, which are being added to all the time.

***

## Devices

### Zigbee

* [SmartThings Power Outlet](http://www.samsung.com/uk/smartthings/sensors-plug-f-app-uk-v2/) (UK & US)
* [SmartThings Multipurpose Sensor](https://www.smartthings.com/gb/products/smartthings-multipurpose-sensor-2016)
* [SmartThings Multipurpose Sensor (2016)](https://www.smartthings.com/gb/products/smartthings-multipurpose-sensor-2016)
* [SmartThings Motion Sensor](https://www.smartthings.com/gb/products/smartthings-motion-sensor)
* [SmartThings Motion Sensor (2016)](https://www.smartthings.com/gb/products/smartthings-motion-sensor-2016)
* [SmartThings Water Leak Sensor](https://www.smartthings.com/gb/products/smartthings-water-leak-sensor)
* [SmartThings Button](https://www.smartthings.com/gb/products/smartthings-button)
* [GE ZB4101 - On/Off Light and Small Appliance Module](https://byjasco.com/products/ge-zigbee-plug-smart-switch)
* [GE ZB3101 - Dimmer Lamp Module](https://byjasco.com/products/ge-zigbee-plug-smart-dimmer)
* [Sylvania SMART+ Smart Plug 729222-A](https://consumer.sylvania.com/our-products/smart/smart-connected-lighting/index.jsp)
* [Sylvania SMART+ ZigBee 2 Button Dimming Switch](https://consumer.sylvania.com/our-products/smart/product-info/zigbee/sylvania-smart-zigbee-2-button-dimming-switch/index.jsp)
* Sylvania SMART+ Adjustable White Bulb (71831)
* [OSRAM Smart+ Plug EU](https://www.osram-lamps.com/ecatalog/smart-home/smart-home-components/smart-plug/index.jsp)
* OSRAM Lightify Tunable White Bulb (73674)
* [OSRAM Outdoor Flex Multicolor](https://smartplus.ledvance.com/products/outdoor-lighting/index.jsp)
* [Cree Bulbs](https://creebulb.com/)
* [IKEA TRÅDFRI Bulbs](https://www.ikea.com/gb/en/products/lighting/smart-lighting/wireless-led-bulbs/)
* [Philips Hue Bulbs](https://www.philips.co.uk/c-m-li/hue) (requires a remote to factory reset)
* [Develco Motion Sensor](https://www.develcoproducts.com/products/sensors-and-alarms/motion-sensor/)
* [Develco Window Sensor](https://www.develcoproducts.com/products/sensors-and-alarms/window-sensor/)
* [Yale YRD226 Deadbolt (Zigbee)](https://www.yalehome.com/en/yale/yalehome/residential/yale-real-living/assure-lock/yrl-assurelock-touchscreen/)
* Many other Zigbee smart plugs, sensors, switches and bulbs using the Home Automation profile should also work. Please add devices you have tested to this wiki page.

### Z-Wave

* [Aeotec Smart Switch 6](https://aeotec.com/z-wave-plug-in-switch)
* [Aeotec Smart Dimmer 6](https://aeotec.com/z-wave-plug-in-dimmer)
* [GoControl Z-Wave Bulb](http://www.gocontrol.com/detail.php?productId=7)
* [Leviton VRPA1 - Plug-in Outlet Module](http://www.leviton.com/en/products/dzpa1-2bw) The VRPA1 apepars to have been suprceded by the DZPA1.
* [GE ZW4103 - Plug-in Smart Switch](https://products.z-wavealliance.org/products/1435)
* [GE 12730 - Fan Control In-Wall Switch](https://byjasco.com/products/ge-z-wave-wall-smart-fan-control)
* [Inovelli NZW31 Dimmer Switch + Repeater](https://inovelli.com/shop/smart-light-switches/z-wave-dimmer-switch-repeater/)
* [Inovelli NZW37 2 Channel Dual Smart Plug](https://inovelli.com/shop/smart-plugs/2-channel-smart-plug/)  Only channel 1 is detected
* [Inovelli OM71V 1 Channel Smart Plug - Outdoor Rated](https://inovelli.com/shop/outdoor-smart-plugs/outdoor-z-wave-plug-in-module-1-channel/) 
* [Devolo Smartmetering Plug](https://www.devolo.com/en/Products/devolo-Home-Control-Smart-Metering-Plug)
* [Neo Coolcam smart power Plug](http://www.szneo.com/en/products/index.php?id=41)
* [Fibaro Wall Plug 2 FGWPF-102-ZW5](https://www.fibaro.com/en/products/wall-plug/)
* Many other Z-wave smart plugs, sensors, switches and bulbs should also work. Please add devices you have tested to this wiki page.

### Wi-Fi

* Broadlink
  * [IR devices](http://www.ibroadlink.com/)
* Eufy
  * [Switches &amp; Plugs](https://www.eufylife.com/collections/smart)
  * [Bulbs](https://www.eufylife.com/collections/led)
* Google
  * [Chromecast](https://store.google.com/category/home_entertainment)
* LIFX
  * [Bulbs](https://www.lifx.com/collections/featured-products#lights)
* Logitech
  * [Harmony Hub](https://www.logitech.com/en-us/product/harmony-hub)
* Netatmo
  * [Weather Station &amp; Modules](https://www.netatmo.com/en-US/product/weather/)
  * [Healthy Home Coach](https://www.netatmo.com/en-US/product/aircare/homecoach)
* Philips Hue
  * [Bulbs](https://www2.meethue.com/en-us/products#filters=STARTER_KITS_SU%2CBULBS_SU%2CLIGHTSTRIPS_SU%2CLAMPS_SU%2CCONTROLS_SU&sliders=&support=&price=&priceBoxes=&page=&layout=12.subcategory.p-grid-icon) -- using Zigbee to Wi-Fi bridge
* Sonos
  * [Speakers](https://www.sonos.com/en-us/shop)
* TP-Link
  * [Bulbs](http://www.tp-link.com/us/home-networking/smart-home/smart-bulbs)
  * [Switches](http://www.tp-link.com/us/home-networking/smart-home/smart-switches)
  * [Plugs](http://www.tp-link.com/us/home-networking/smart-home/smart-plugs)
* Wemo
  * [Switches &amp; Plugs](http://www.wemo.com/products/)
* Yeelight
  * [Bulbs](https://www.yeelight.com/)

Bluetooth
* See 'HomeKit' and 'Other' below

### HomeKit

Most WiFi and BLE devices should work. Below is a list of _tested_ devices.

**Note**: Devices typically need to be unpaired from iOS (or their Android app) before using them with the gateway.

* Smart plugs
  * [iDevices Switch](https://store.idevicesinc.com/idevices-switch/)
  * [Koogeek P1](https://www.koogeek.com/p-p1.html)
  * [Eve Energy](https://www.evehome.com/en/eve-energy)
* Bridges
  * [Homebridge](https://github.com/nfarina/homebridge)
  * [Philips Hue Bridge v2](https://www2.meethue.com/en-us/p/hue-bridge/046677458478)
* Light bulbs
  * [LIFX A19](https://www.lifx.com/products/lifx)
* Sensors
  * [Eve Door &amp; Window](https://www.evehome.com/en/eve-door-window)
  * [Eve Motion](https://www.evehome.com/en/eve-motion)

### Other

* Raspberry Pi GPIO
* Raspberry Pi I<sup>2</sup>C
* Wi-Fi developer hardware
  * Espressif [ESP8266](https://www.espressif.com/en/products/hardware/esp8266ex/overview) and [ESP32](https://www.espressif.com/en/products/hardware/esp32/overview)
  * Microchip [SAMD21](https://www.microchip.com/wwwproducts/en/ATsamd21g18) MCUs with [WINC1500](https://www.microchip.com/wwwproducts/en/ATwinc1500) module
    * [MKR1000](https://store.arduino.cc/usa/arduino-mkr1000)
    * [SAMW25 Xplained Pro](https://www.microchipdirect.com/product/search/all/ATSAMW25-XPRO)
* Bluetooth developer hardware
  * Nordic [nRF52 Developer Kit](https://www.nordicsemi.com/eng/Products/Bluetooth-low-energy/nRF52-DK), [Thingy52](https://www.nordicsemi.com/eng/Products/Nordic-Thingy-52), ...
  * Nordic nRF51 BBC [micro:bit](https://microbit.org/) (using [MicroBlocks](http://microblocks.fun/) [VM](http://microblocks.fun/tech.html#techspecs) on board connecting to [MicroBlocks add-on](https://github.com/mozilla-iot/microblocks-adapter) on gw)
* [Arduino](https://github.com/mozilla-iot/webthing-arduino)
* [Microblocks](http://microblocks.fun/)
* [Serial devices](https://github.com/mozilla-iot/serial-adapter)
* [Pimoroni Blinkt!](https://shop.pimoroni.com/products/blinkt)
* [X10](https://www.x10.com/)