This page documents a typical setup that a developer on our team would use:

# Raspberry Pi 3

The raspberry Pi 3 includes bluetooth and wifi, so it is generally preferred over the earlier models, but any model that has access to your network should work.

# ZigBee

We currently only support the [Digi XStick2 ZB](https://www.digi.com/products/xbee-rf-solutions/boxed-rf-modems-adapters/xstick) dongle. This dongle has a part number of XU-Z11 and can be purchased from [Mouser](http://www.mouser.com/search/ProductDetail.aspx?R=0virtualkey0virtualkeyXU-Z11). I looks like DigiKey doesn't stock this any more. It's IMPORTANT that you get an "XSTICK2 ZB" and NOT an "XSTICK1 802.15.4".

In North America, the [GE Smart Dimmer](https://byjasco.com/products/ge-zigbee-plug-smart-dimmer) [GE Smart Switch](https://byjasco.com/products/ge-zigbee-plug-smart-switch) and [Centralite 4256060](https://www.amazon.com/CentraLite-Lighting-Control-Zigbee-4256050-ZHAC/dp/B00IUMU1RM) are known to work.

# ZWave

Most ZWave dongles should work. We've used the [AEOTEC Z-Stick Gen5](https://www.amazon.com/Aeotec-Aeon-Labs-ZW090-Stick/dp/B00X0AWA6E), although the [UZB](http://www.zwaveproducts.com/shop/controllers/z-wave-software-controllers/z-wave-plus-usb-controller) will fit better on a Raspberry Pi. If you do choose to get the AEOTEC Z-Stick, then you may also want to use a short [USB extension cable](https://www.amazon.com/Pack-15cm-Adjustable-Flexible-Extension/dp/B01GA1GKYW/)

Leviton products also seem to work well.

# Lights

I normally use inexpensive night lights to plug into the ZWave/ZigBee outlets for testing. These can typically be purchased at a local dollar store. If you get one with a light sensor, then you'll need to cover the sensor with eletrical tape so that it thinks its always dark.

The [AEOTEC Smart Switch 6](http://aeotec.com/z-wave-plug-in-switch) available from [Amazon](https://www.amazon.com/Aeotec-Aeon-Labs-ZW096-Switch/dp/B00VQISOCG/) works well.


