|`cordova-air-update-server`|`cordova-air-update-cli`|`cordova-plugin-air-update`|
|---|---|---|
|[![npm version](https://badge.fury.io/js/cordova-air-update-server.svg)](https://badge.fury.io/js/cordova-air-update-server)|[![npm version](https://badge.fury.io/js/cordova-air-update-cli.svg)](https://badge.fury.io/js/cordova-air-update-cli)|[![npm version](https://badge.fury.io/js/cordova-plugin-air-update.svg)](https://badge.fury.io/js/cordova-plugin-air-update)|

## Note: It's really early alpha version

## What is AirUpdate?

That set of `air update` parts will provide you a possibility to build your own app publisher/deployer
on you private invirement for free. The functionality is similar to Ionic Deploy / Microsoft Code Push which let you publish web assets such as HTML, JS, and CSS directly to your users without going through the app store.

## Booom!
- CLI pushes only delta's to the server.
- Server pushes only delta's to the app.

So there is no big updates with all havy assets, just only the deltas travaling to/from server.

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

#### commands
The commands shuld be executed in the cordova project folder (where you have your www, platforms, etc... folders)
* `cau init` - config initialization
* `cau login` - login with AirUpdate server
* `cau logout` - logout...
* `cau app add` - add your application to the AirUpdate server
* `cau platform add <platform>` - for no it's only `ios` supported
* `cau channel add <platform> <channelKey>` - add channel to the app. Ex `dev`
* `cau deploy <platform> <channelKey>` - deploy the app version to the channel. If channel is not provided, 
then default one will be used

Extra (custom) data to version release can be added by adding keys with prefix `--extra.` to deploy command.<br>
Example:<br>
To receive on app `extras` data json like
``` json
{
  "canNotSkipVersion": true,
  "message": "That hot release fixes some security issues"
}
```
Run command `cau deploy <platform> <channelKey> --extra.canNotSkipVersion=true --extra.message="That hot release fixes some security issues"`

### AirUpdate Cordova Plugin

The native part of the plugin is used only to check if there is installed newer version on the app on the 
device and reload webview with new path to index.html.

#### Plugin (JS) docs:
`AirUpdate.init(<code>)` method to initialize the plugin. It's possible to pass a parameter (ex: `AirUpdate.init('dev')` if the channel to set active channel of AirUpdate.<br>
Returns: `Promise`

`AirUpdate.setChannel(code)` set active channel of AirUpdate.<br>
Returns: `boolean` true if successfuly set channel.

`AirUpdate.getLatestServerVersion()` retrive latest meta version from `AirUpdate` server.<br>
Returns: `Promise<IMeta>` can resolve with JSON object or with null. Resolved with `null` only of server is not accessable or there is if nothing has been pushed yet to server channel.

`AirUpdate.getCurrentLocalVersion()` get current local version meta.<br>
Returns: `Promise<IMeta>`

`AirUpdate.downloadVersion(version)` download spicific version files from `AirUpdate` server. (Ex: `AirUpdate.downloadVersion('1.2.1')`<br>
Returns: `Promise<void>`

`AirUpdate.setupVersion(version)`the method creates and installes to `'www'` folder files of selected version. The changes are not visible yet for the user. User will see the updated version after app reload or after calling `AirUpdate.reload()`.<br>
Returns: `Promise<void>`

`AirUpdate.getAllLocalVersions()`<br>
Returns: `Promise<string[]>` list of all available on the device versions for the active channel

`AirUpdate.deleteLocalVersions(verions: string[])` *Not Implemented yet* Will delete meta info and related files from the device.<br>
Returns: `Promise<void>`

`AirUpdate.reload()` Reaload the app with new update. (without exiting the app)<br>
Returns: `Promise<void>`

`AirUpdate.clearWWWs()` removed unised app instances (does not removes apps versions). It's kind of clearing useless data.

#### Dependencies
* cordova-plugin-file
* cordova-plugin-advanced-http
* cordova-plugin-zip

#### IMeta
```js
{
  version: string; // version number
  filesMap: {[prop: string]: string} // it's map of all files used on the version and there's crc (md5)
  extras: any;
}
```

## TODO
### Common
* Delta's from server to client
* ~~Add extra meta data (custom) to the release (deploy)~~
* Revoke specific version
* Standalone server with https
