This page describes how to enable some debug features to get more detailed information about the frames being sent and received.

You'll need to edit one of the files from the zigbee adapter. The first step is to locate the files. Prior to 0.4 the addons will be found in the build/addons directory located inside the gateway source tree. On the Raspberry Pi, this would be `/home/pi/mozilla-iot/gateway/build/addons`. From 0.4 onwards the addons will be found inside `~/.mozilla-iot/addons` (note the dot at the beginning of .mozilla-iot). On the Raspberry Pi this would be `/home/pi/.mozilla-iot/addons`

Once you've located the addons directory, you should see a zigbee-adapter directory containing the files for the zigbee adapter. Since we're going to be editing the files, we need to disable the integrity checking. The simplist way is to create a `.git` subdirectory inside the zigbee-adapter directory. A second way is to remove the `SHA256SUMS` entry from the `files` key inside the `package.json` file.

Now that you've disabled the integrity check, you can edit the source files. To enable debugging, edit the zb-adapter.js file and seach for the first occurrence of debugFrames. You should see something like this:
```js
    // debugFrames causes a 1-line summary to be printed for each frame
    // which is sent or received.
    this.debugFrames = false;

    // debugFrameDetail causes detailed information about each frame to be
    // printed.
    this.debugDumpFrameDetail = false;
```
Setting `debugFrames` to true will cause a 1-line summary to be printed. Setting `debugFrameDetail` to true will cause the full frame to be printed.

Now go into Settings->Add-ons, disable and re-enable the zigbee adapter and you should start to see information about each frame being sent and received. On the Raspberry Pi, you can find the log files in `/home/pi/.mozilla-iot/log/run-app.log` for 0.4 and newer and in `/home/pi/mozilla-iot/gateway/run-app.log` for 0.3.1 and earlier.