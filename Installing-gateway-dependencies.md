# Install pkg-config (OSX)

[This page](http://macappstore.org/pkg-config/) covers installing pkg-config, which is required to build openzwave.

# Install openzwave

[This page](https://github.com/moziot/wiki/wiki/Installing-openzwave) covers installing openzwave for OSX and Linux.

# Install nvm

nvm allows you to easily switch between different versions of node quite easily.
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```
Close the terminal window and reopen.

Verify nvm is in your path by typing the command `nvm` You should see the help output.
```
nvm install --lts
nvm use --lts
nvm alias default --lts
```
Verify:

```
some-prompt> npm --version
5.6.0

some-prompt> node --version
v8.11.1
```