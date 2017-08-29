|`cordova-air-update-server`|`cordova-air-update-cli`|`cordova-plugin-air-update`|
|---|---|---|
|[![npm version](https://badge.fury.io/js/cordova-air-update-server.svg)](https://badge.fury.io/js/cordova-air-update-server)|[![npm version](https://badge.fury.io/js/cordova-air-update-cli.svg)](https://badge.fury.io/js/cordova-air-update-cli)|[![npm version](https://badge.fury.io/js/cordova-plugin-air-update.svg)](https://badge.fury.io/js/cordova-plugin-air-update)|

## Note: It's really early alpha version

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

Plugin used to replace original `index.html` with updated one during app start (before original page is displayed). See the details below.

## Platforms supported
* ios
* android (in the future)
* windows (in the future)

## Installation of all set

### Server side
I think it's better to start with server side (I did not test that with Windows).

I consider that you alredy have installed `node.js` and `MongoDB`.

The server need's 3 folders
* applications repository folder to keep app files (html, js....) `pathApps`
* tmp folder to keep temporary uploads `pathTemp`
* cache folder to keep prepared zip archives for clients `pathCache`

So, installation and start by steps:
1. install `npm i -g cordova-air-update-server`
2. create your config json file (see an example below)
3. start `cau-server -o <path to your config file>`

Pretty simple, no?

#### Server config example (full)
``` js
{
  "apiServer": {
    "port": 3000, // default server port
    "apiPathPrefix": "/api" // default api prefix
  }
  "mongo": {
    "server": "mongodb://user:password@address:port/dbName", // mongoDB connection path
    "options": {
      "server": {
        "reconnectTries": 999999999999
      }
    }
  },
  "pathApps": "/home/airUpdate/apps",
  "pathTemp": "/home/airUpdate/uploads",
  "pathCache": "/home/airUpdate/cache"
}

```


### Command line tool (CLI)
So, the installation is also pretty simple, just run `npm i -g cordova-air-update-cli`.

### commands
* `cau init` - config initialization
* `cau login` - login with AirUpdate server
* `cau logout` - logout...
* `cau app add` - add your application to the AirUpdate server
* `cau platform add <platform>` - for no it's only `ios` supported
* `cau channel add <platform> <channelKey>` - add channel to the app. Ex `dev`
* `cau deploy <platform> <channelKey>` - deploy the app version to the channel. If channel is not provided, 
then default one will be used

