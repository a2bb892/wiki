There is one code change required on your local Gateway to test the prerelease OTA update.

Change https://github.com/mozilla-iot/gateway/blob/master/config/default.js#L69, replacing the api.mozilla-iot.org URL with `https://api.github.com/repos/hobinjk/gateway/releases`
to make it look like:
```javascript
  updateUrl: 'https://api.github.com/repos/hobinjk/gateway/releases',
 ```
On the RPi
 `cd /home/pi/mozilla-iot/gateway/config` 
to edit default.js as described above.

Now run `sudo systemctl restart mozilla-iot-gateway.service` to make sure the code changes are propagated

Finally, visit https://your-gateway.mozilla-iot.org/settings/updates to update your gateway using OTA. Please report any issues you encounter with this process [on GitHub](https://github.com/mozilla-iot/gateway/issues). Note that this process does take around 7-10 minutes to complete depending on your Raspberry Pi's processor power and time it takes to download the update.
