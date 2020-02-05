# Installing Node.js
## Download from nodejs.org

You can download and install a binary from [nodejs.org](https://nodejs.org).

## nvm

Alternatively nvm allows you to easily install different versions of node. To install nvm:

```
 $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```

Close and reopen your terminal window. Use nvm to install node and set the default version.

```
$ nvm install --lts
$ nvm use --lts
$ nvm alias default --lts
```

Verify that node and npm have been installed:
```
$ npm --version
5.6.0
$ node --version
v8.11.1
```

## HomeBrew
On Mac OS X you can install Node.js using [HomeBrew](https://brew.sh/)

```
$ brew install node
```