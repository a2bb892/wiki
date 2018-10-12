This is the procedure I've used when the initial pairing doesn't work.

This assumes that you're using a SmartThings sensor which has a connect button (small white button visible typically when removing the cover).

1 - Try resetting the sensor. Press and hold the connect button. If the LED immediately changes to a greenish orange color then continue to press and hold the button for 5-6 seconds until you see it flash red. The LED should then start flashing blue to indicate that it's looking for a network.
2 - If the LED didn't immediately change to greenish orange in Step 1, then remove the battery. Press and hold the connect button while inserting the battery. Continue holding the button for 5-6 seconds until the LED changes color.
3 - If the LED flashed green then the sensor should be repaired and should start to work (assuming it was previously paired and added to the gateway)
4 - If the LED is flashing blue, then press + in the UI to add new devices. This puts the zigbee dongle into pairing mode. If the sensor was previously added then you won't see a new device show up. If you see the LED flashing green then you can cancel out of the pairing mode and wait for a minute and it should start working again. If the sensor wasn't previously added, then you should see it show up. If the pairing process times out, press + again.