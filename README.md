# cordova-plugin-ftp

## Description

This cordova plugin is created to use ftp (client) in web/js.

Support both **iOS** and **Android** platform now.

You can do the following things:

- List a directory
- Create a directory
- Delete a directory (must be empty)
- Delete a file
- Download a file (with percent info)
- Upload a file (with percent info)
- Cancel upload/download

## Installation

```sh
$ cordova plugin add cordova-plugin-ftp
$ cordova prepare
```

Dependency:

- For iOS, the plugin depends on *CFNetwork.framework*, which has been added to plugin.xml (and `cordova prepare` will add it to platfrom project), so you don't need to do anything.
- But for Android, it depends on *com.android.support:support-v4:23.2.0*, which should be added to your platfrom project by hand.

## Usage

You can access this plugin by js object `window.cordova.plugin.ftp`.

Example:

```js
document.addEventListener("deviceready", onDeviceReady, false);

function onDeviceReady() {
    window.cordova.plugin.ftp.connect('address(ip/domainname)', 'username', 'password', function(ok) {
        console.info("ftp: connect ok=" + ok);

        // You can do any ftp actions from now on...
        window.cordova.plugin.ftp.upload('/localPath/localFile', '/remotePath/remoteFile', function(percent) {
            if (percent == 1) {
                log.info("ftp: upload finish");
            } else {
                log.debug("ftp: upload percent=" + percent * 100 + "%");
            }
        }, function(error) {
            log.error("ftp: upload error=" + error);
        });

    }, function(error) {
        console.error("ftp: connect error=" + error);
    });
}
```

- Refer to [ftp.js](./www/ftp.js) for all available APIs.

## Notice

1. For iOS, `ftp.connect` will always succeed (even if `username` and `password` are incorrect), but it does NOT mean the later actions, e.g. `ls`... `download` will succeed too! So always check their `errorCallback`.
2. Want to upload/download multiple files? The plugin (Android part) just inits one connection and transmits all files via that connection. If you use asychronous syntax (e.g. `foreach`) to start multiple upload/download in a short time, it may mess the transfer. Instead, you can try [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) or [async](https://github.com/caolan/async) to transmit one after one.

## Thanks

- The iOS native implementing is based on [GoldRaccoon](https://github.com/albertodebortoli/GoldRaccoon).
- The Android native implementing is based on [ftp4j](http://www.sauronsoftware.it/projects/ftp4j/) jar (LGPL).

