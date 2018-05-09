# Pycharm Setup

***


This section contains information for developers used to using the [Jet Brains](https://www.jetbrains.com/). Specifically Pycharm - which actually works perfectly well with 'JavaScript' like languages. 

## Prerequisites
*You have cloned or forked [mozilla-iot gateway](https://github.com/mozilla-iot/gateway)
*You have installed a version of [Pycharm 2017.1](https://www.jetbrains.com/pycharm/?fromMenu) or later.
*Pycharm as the plugin Node.js integration, Vendor-JetBrains at version 171 or later.


## Using The Correct Lint Tool

The Lint tool used for the WoT project should be in your /node_packages directory here:
   /> workspace/WoT_Gateway/gateway/node_modules/eslint

To run the lint tool from the command line you could do:

   /> npm run lint
or:
   /> ./node_modules/eslint/bin/eslint.js .

## Setting Pycharm To Use The Lint Tool

But you don't want to be bothered with all that hassle if you have a good IDE! 
From the pycharm settings directory: file -> settings

From the settings dialogue box, select: Languages & Frameworks -> JavaScript -> Code Quality Tools -> Eslint.
Ensure that:
* Enabled is checked
* ESLing package is set as: ~/<your workspace>/<your directory>/gateway/node_modules/eslint
* Select OK.





