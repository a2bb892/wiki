If you're doing lots of random testing and generating lots of new domains or other weird things, you can switch to the staging server. The staging database gets wiped periodically, so it's not a huge issue to do strange things.

To use the server, modify the following:
* `config/default.js`: In the `ssltunnel` section, change the following:
  * `registration_endpoint: 'https://api.mozilla-iot-staging.org:8443',`
  * `domain: 'mozilla-iot-staging.org',`
  * `certemail: 'certificate@mozilla-iot-staging.org',`
* `src/certificate-manager.js`: Search for `production` and replace it with `staging`