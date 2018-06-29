Run the following code in a directory with express and simple-oauth2 installed. You can then visit http://localhost:31338/auth to initiate an OAuth flow that will get your credentials sent to http://localhost:31338/callback. This "test" client is set up in `src/models/oauthclients.ts` and can be modified to suit whichever scenario you want to test.

```javascript
process.env.NODE_TLS_REJECT_UNAUTHORIZED = '0';

const express = require('express');
const http = require('http');
const simpleOAuth2 = require('simple-oauth2');

const CLIENT_ID = 'test'; // 'hello' on 0.3
const CLIENT_SECRET = 'super secret';
const REQUEST_SCOPE = '/things:readwrite'; // 'readwrite' on 0.3
const REQUEST_STATE = 'somethingrandom';

const config = {
  client: {
    id: CLIENT_ID,
    secret: CLIENT_SECRET
  },
  auth: {
    tokenHost: 'https://127.0.0.1:4443/oauth'
  }
};

const oauth2 = simpleOAuth2.create(config);

const client = express();
const port = 31338;

let jwt;

client.get('/auth', (req, res) => {
  res.redirect(oauth2.authorizationCode.authorizeURL({
    redirect_uri: `http://127.0.0.1:${port}/callback`,
    scope: REQUEST_SCOPE,
    state: REQUEST_STATE
  }));
});

client.get('/callback', (req, res) => {
  const code = req.query.code;
  console.log('query', req.query);

  oauth2.authorizationCode.getToken({code: code}).then(result => {
    const token = oauth2.accessToken.create(result);
    jwt = token;
    // At this point you can make requests to the gateway using Authorization: Bearer {jwt}
    res.json(token);
  }).catch(err => {
    res.status(400).json(err);
  });
});

const clientServer = http.createServer();
clientServer.on('request', client);
clientServer.listen(port);
```

The exact definition of the `test` client is the following code in `src/models/oauthclients.ts`:
```javascript
let oauthClients = new OAuthClients();
oauthClients.register(
  new ClientRegistry(new URL('http://127.0.0.1:31338/callback'), 'test',
                     'Test OAuth Client', 'super secret', '/things:readwrite')
);
```

The 127.0.0.1 addresses assume that the same device is running the gateway and the simpleOAuth2 server. If you want to run these on separate devices, the first relevant place to make changes is https://127.0.0.1:4443 to point to where you're hosting the gateway, e.g. https://gateway.local, https://192.168.0.103, or https://your-gateway.mozilla-iot.org. The next is http://127.0.0.1:31338/callback which should point to wherever you're running the simpleOAuth2 server described in the first source code listing. This could be http://192.168.0.104:31338/callback or http://somedomainyouown.com/callback.

It would also be recommended to change 'test' to a new client id, 'Test OAuth Client' to a description of your service, and 'super secret' to a randomized secret.
