// Cordova iOS Pushwoosh Push Notifications plugin
// (c) Pushwoosh, 2012

0. Visit http://www.pushwoosh.com and create an account there. Create the application and configure it (or you can configure it later).

1. Drag and drop the "PushNotification" folder from Finder to your Plugins folder in XCode (use "Create groups for any added folders")

2. Add the PushNotification.js file to your "www" folder on disk

3. Add the reference to the .js file using `<script>` tags in your html file(s):
	<script type="text/javascript" charset="utf-8" src="PushNotification.js"></script>

4. Add new entry with key `PushNotification` and value `PushNotification` to `Plugins` in "Cordova.plist/Cordova.plist"

5. Add cp.pushwoosh.com to <strong>hosts</strong> in Cordova.plist/ExternalHosts (or you can simply add wildcard *).

6. See the index.html sample for the Javascript integration.
To register for push notifications you have to call:
(Replace PUSHWOOSH_APP_CODE with your Application Id from Pushwoosh and APP_NAME with your Application name)

var pushNotification = window.plugins.pushNotification;
pushNotification.registerDevice({alert:true, badge:true, sound:true, appid:"PUSHWOOSH_APP_CODE", appname:"APP_NAME"},
                                function(status) {
                                    console.warn('registerDevice:%o', status);
                                    navigator.notification.alert(JSON.stringify(['registerDevice', status]));
                                },
                                function(status) {
                                console.warn('failed to register :%o', status);
                                navigator.notification.alert(JSON.stringify(['failed to register ', status]));
                                });


NOTE: Push notifications must be handled in two places:

a. In the onBodyLoad function. This method gets called when push notification has been received.
document.addEventListener('push-notification', function(event) {
							console.warn('push-notification!: ' + event.notification);
							navigator.notification.alert(JSON.stringify(['push-notification!', event.notification]));
						  });

b. In the onDeviceReady function. This code gets called when the application has been started in response to the push notification.

if(typeof(invokeString) != "undefined" && invokeString.length > 0 &&  invokeString[0] == '{') {
	//push notification
	console.warn('push-notification!: ' + invokeString);
	navigator.notification.alert(JSON.stringify(['push-notification!', invokeString]));
}

7. See PushNotification.js for more information on the interface

8. Wasn't it TOO EASY?

P.S. You might want to ask - "Do I have to change AppDelegate.m file as for all other PN plugins?".
No! You have to do only the steps mentioned above. That's where the magic comes true.