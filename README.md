## What is AirUpdate?

That set of `air update` parts will provide you a possibility to build your own app publisher/deployer
on you private invirement for free. The functionality is similar to Ionic Deploy / Microsoft Code Push which let you publish web assets such as HTML, JS, and CSS directly to your users without going through the app store.

## Set contains

### Server side `cordova-air-update-server`

`npm i -g cordova-air-update-server`
Provides server side written with typescript for node.js. The server provides api for cordova plugin and CLI tool.
It's really simple to install it (see below).


### CLI tool `cordova-air-update-cli`

`npm i -g cordova-air-update-cli`
Provides easy way to prepare and push new versions to the server's specific channel. Installation creates links to
use cli with `cordova-air-update ... ` or `cau ... `


### Cordova Plugin `cordova-plugin-air-update`

Cordova: `cordova plugin add cordova-plugin-air-update --save`

Ionic: `ionic cordova plugin add cordova-plugin-air-update`

Plugin used to replace original `index.html` with updated one during app start (before original page is displayed).
