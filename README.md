# Firebase Cloud Messaging - Titanium Module
Use the native Firebase SDK in Axway Titanium. This repository is part of the [Titanium Firebase](https://github.com/hansemannn/titanium-firebase) project.

## Requirements
- [x] Titanium SDK 6.2.0 or later

## Download
- [x] [Stable release](https://github.com/hansemannn/titanium-firebase-cloud-messaging/releases)
- [x] [![gitTio](http://hans-knoechel.de/shields/shield-gittio.svg)](http://gitt.io/component/firebase.cloudmessaging)

## API's

### `FirebaseCloudMessaging`

#### Methods

##### `registerForPushNotifications()` (iOS)
##### `registerForPushNotifications({onToken: function(e){}, onMessage: function(e){} })` (Android)

##### `appDidReceiveMessage(parameters)`
  - `parameters` (Dictionary)
  
##### `sendMessage(parameters)`
  - `parameters` (Dictionary)
    - `message` (String)
    - `messageID` (String)
    - `to` (String)
    - `timeToLive` (Number)
  
##### `subcribeToTopic(topic)`
  - `topic` (String)

##### `unsubcribeFromTopic(topic)`
  - `topic` (String)
  
#### Properties

##### `fcmToken` (String, get)

##### `apnsToken` (String, set)

##### `shouldEstablishDirectChannel` (Number, get/set)

#### Events (iOS)

##### `didReceiveMessage`
  - `message` (Dictionary)
  
##### `didRefreshRegistrationToken`
  - `fcmToken` (String)

## Example
```js
var fcm = require('firebase.cloudmessaging');
fcm.registerForPushNotifications({
    onToken: function(e){
        // get token from callback
        Ti.API.info("Token: " + e.token);
    },
    onMessage: function(e){
        // got message
        Ti.API.info("Message", e.message);
    }
});

if (OS_IOS){
    // get token from callback
    fcm.addEventListener("didRefreshRegistrationToken", function(e){
        Ti.API.info("Token: " + e.token);
    });

    // got message
    fcm.addEventListener("didReceiveMessage", function(e){
        Ti.API.info("Message", e.message);
    });
}

// check if token is already available
if (fcm.fcmToken != null){
    Ti.API.info('FCM-Token: ' + fcm.fcmToken);
} else {
    Ti.API.info('Token is empty. Wait for the token callback.');
}

// subscribe to topic
fcm.subcribeToTopic("testTopic");
```

## Send FCM messages with PHP
To test your app you can use this PHP script to send messages to the device/topic:

```php
<?php $url = 'https://fcm.googleapis.com/fcm/send';

	$fields = array (
			'to' => "/topics/testTopic", // or device token
			'notification' => array (
					"title" => "TiFirebaseMessaging",
					"body" => "Message received"
			)
	);

	$headers = array (
			'Authorization: key=SERVER_ID_FROM_FIREBASE_SETTIGNS_CLOUD_MESSAGING',
			'Content-Type: application/json'
	);

	$ch = curl_init ();
	curl_setopt ( $ch, CURLOPT_URL, $url );
	curl_setopt ( $ch, CURLOPT_POST, true );
	curl_setopt ( $ch, CURLOPT_HTTPHEADER, $headers );
	curl_setopt ( $ch, CURLOPT_RETURNTRANSFER, true );
	curl_setopt ( $ch, CURLOPT_POSTFIELDS, json_encode($fields));

	$result = curl_exec ( $ch );
	echo $result;
	curl_close ( $ch );
?>
```

Run it locally with `php filelane.php` or put it on a webserver where you can execute PHP files.

## Build
```js
cd ios
appc ti build -p ios --build-only
```

## Legal

This module is Copyright (c) 2017-Present by Appcelerator, Inc. All Rights Reserved. 
Usage of this module is subject to the Terms of Service agreement with Appcelerator, Inc.  
