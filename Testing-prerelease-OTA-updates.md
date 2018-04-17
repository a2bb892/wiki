There are two code changes required on your local Gateway to test the prerelease OTA update.

Remove `!release.prerelease` https://github.com/mozilla-iot/gateway/blob/master/src/controllers/updates_controller.js#L79
to make it look like:
```javascript
 return !release.draft;
 ```
 
Remove the same `!release.prerelease` from https://github.com/mozilla-iot/gateway/blob/master/tools/check-for-update.js#L16
making it look like:
 ```javascript
 return !release.draft;
 ```

Now run `sudo systemctl restart mozilla-iot-gateway.service` to make sure the code changes are propagated

Finally, visit https://your-gateway.mozilla-iot.org/settings/updates to update your gateway using OTA. Please report any
issues you encounter with this process [on GitHub](https://github.com/mozilla-iot/gateway/issues). Note that this process
does take around seven minutes to complete depending on your Raspberry Pi's speed.
