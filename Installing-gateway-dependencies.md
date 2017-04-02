# Install pkg-config (OSX)

[This page](http://macappstore.org/pkg-config/) covers installing pkg-config, which is required to build openzwave.

# Install openzwave

[This page](https://github.com/moziot/wiki/wiki/Installing-openzwave) covers installing open-zwave for OSX and Linux.

# Recommended - Insall nvm

nvm allows you to easily switch between different versions of node quite easily.
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```
Close the terminal window and reopen.

Verify nvm is in your path by typing the command `nvm` You should see the help output.
```
nvm install v7.7.2
nvm use v7.7.2
nvm alias default v7.7.2
```
Verify:

```
some-prompt> npm --version
4.1.2

some-prompt> node --version
v7.7.2
```