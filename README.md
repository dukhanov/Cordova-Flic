# Cordova-Flic
A Cordova plugin providing access to the Flic SDK (Android and iOS)

## Installation

```
    $ cordova plugin add cordova-plugin-flic --variable URL_SCHEME=flic20
```

### Android

Set `android:minSdkVersion="19"` or higher in config.xml for the Android
```xml
	<preference name="android-minSdkVersion" value="19" />
```

	$ cordova build android

### iOS

Set `deployment-target=8.0` or higher in config.xml for the iOS 

```xml
	<preference name="deployment-target" value="8.0" />
```

Make sure, that you have installed [node-xcode](https://www.npmjs.com/package/xcode) version 0.8.7 or higher on your Mac

```
$ npm i xcode
```

	$ cordova build ios

## Plugin API
It has been currently stripped to the minimum needed from a Javascript app.

The following functions are available:

* Flic.init (config, success, error). Initialize Flic and register known buttons for receiving single click, double click and hold events
  * config:
	* appId: your app client ID
	* appSecret: your app client secret
	* appName: your app name
	* reloadOnFlicEvent: in case you want to start the App via Flic event (Android only, Boolean, default: false)
  * success: called on function success
  * error: called on function error
* Flic.getKnownButtons(success, error). Get known buttons. Returns the list of buttons grabbed in a previous run of the app
  * success: called on function success
  * error: called on function error
* Flic.grabButton(success, error). Grab a button and register it for receiving single click, double click and hold events. Returns the grabbed button
  * success: called on function success
  * error: called on function error
* Flic.onButtonClick(onButtonPressed, onButtonPressedError)
  * onButtonPressed: called when flic button is pressed
  * onButtonPressedError: called when flic button is pressed, but some error has occured

## Sample usage code
```Javascript
function successInit(result) {
    console.log('Flic init succeeded');

    // Get known buttons
    Flic.getKnownButtons(
        function(buttons) {
            console.log('Flic getKnownButtons succeeded');
            console.log('Flic known buttons: ' + JSON.stringify(buttons));
        },
        function(message) {
            console.log('Flic getKnownButtons failed: ' + message);
        });
}

function errorInit(message) {
    console.log('Flic init failed: ' + message);
}

function onFlicButtonPressed(result) {
    console.log(result.event); // (String) singleClick or doubleClick or hold
    console.log(result.button.buttonId); // (String)
    console.log(result.button.color); // (String) green
    console.log(result.wasQueued); // (Boolean) If the event was locally queued in the button because it was disconnected. After the connection is completed, the event will be sent with this parameter set to true.
    console.log(result.timeDiff); // (Number) If the event was queued, the timeDiff will be the number of seconds since the event happened.
}

function onFlicButtonPressedError(err){
    console.log(err);
}

var config = {
    appId: 'your app id',
    appSecret: 'your app client secret',
    appName: 'your app name',
    reloadOnFlicEvent: true,
}

// Init flic
Flic.init(config, successInit, errorInit);

// Subscription to button events
Flic.onButtonClick(onFlicButtonPressed, onFlicButtonPressedError)
```
