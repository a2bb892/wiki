# Installing NodeJS
## Download from nodejs.org

You can download and install a binary from [nodejs.org](https://nodejs.org).

## nvm

Alternatively nvm allows you to easily install different versions of node. To install nvm:

```
 $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```

Close and reopen your terminal window. Use nvm to install node and set the default version.

```
$ nvm install v7.7.2
$ nvm use v7.7.2
$ nvm alias default v7.7.2
```

Verify that node and npm have been installed:
```
$ npm --version
4.1.2
$ node --version
v7.7.2
```

## HomeBrew
On Mac OS you can install NodeJS using [HomeBrew](https://brew.sh/)

```
$ brew install node
```