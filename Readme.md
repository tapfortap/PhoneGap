# Tap for Tap PhoneGap Plugins

Want to use Tap for Tap with PhoneGap? We got you covered on Android and iOS.

## Installation
If you don't have the plugins yet then head over to the [downloads page](http://tapfortap.com/developers/sdk) and
get yourself a zip file. Current versions support PhoneGap 2.x.x.

Installing the Tap for Tap plugin is super easy. We'll guide you through it.
This isn't a PhoneGap tutorial so we assume that you have a PhoneGap project
(or projects) already set up and working.

### Android
1. [Download our Android SDK](http://tapfortap.com/developers/sdk) and find
the file `TapForTap.jar` included therein. You'll need this file in the next
step.

2. Peek inside the `Android` folder and put `TapForTapPhoneGapPlugin.jar` in your
project's `libs` folder, alongside `cordova-2.x.x.jar`.
Put `TapForTap.jar` from step 1 to your `libs` folder as well.

3. Now add Tap for Tap to your project by following [Steps 1 and 2 of our SDK instructions](http://tapfortap.com/developer#documentation).

4. Now go ahead and put `tapfortap.js` in `assets/www` alongside
`cordova-2.x.x.js`.

5. Open `res/xml/plugins.xml` and add this line:

```xml
<plugin name="TapForTap" value="com.tapfortap.phonegap.TapForTapPhoneGapPlugin"/>
```

That's it! Unless you're integrating Tap for Tap into an iOS app as well you
can skip down to **Usage**.

### iOS
1. [Download our iOS SDK](http://tapfortap.com/developers/sdk) and find the
folder `TapForTap` included therein. Add it to your project by following
[Step 1 and 2 of the iOS instructions](http://tapfortap.com/developer#documentation).

2. Add `TapForTapPhoneGapPlugin.h` and `TapForTapPhoneGapPlugin.m` to your
project in the `Plugins` folder provided by PhoneGap.

3. Put `tapfortap.js` in your `www` folder, alongside `phonegap-2.x.x.js`
or `cordova-2.x.x.js`.

4. Under `Supporting Files` open `Cordova.plist`. In the  `Plugins` dictionary 
add the key `TapForTap` with value `TapForTapPhoneGapPlugin`. In the 
`ExternalHosts` array add a '*'. This allows TapForTap to talk to our servers and fetch ads from our CDN.

That's it! Now you're ready to use Tap for Tap in your app.

## Usage
First up include the Tap for Tap JavaScript module, this goes directly after
the similar line that includes `phonegap/cordova-2.x.x.js`:

```html
<script type="text/javascript" charset="utf-8" src="tapfortap.js"></script>
```

Second, and lastly, create an ad view and tell it to load ads, like so:

```javascript
document.addEventListener('deviceready', function() {

  var apiKey = '<YOUR API KEY>';
  TapForTap.initializeWithApiKey(apiKey);
  TapForTap.createAdView({}, function() {
    TapForTap.loadAds(function() {
      // successfully loaded ads
    }, function(e) {
      console.error('error loading ads: ', e);
    });
  }, function(e) {
    console.error('error creating ad view: ', e);
  });

}, false);
```

Congratulations, you are done. We said it was easy!

## API Documentation
The JavaScript API lets you create, position, and remove Tap for Tap ad views. You
can also pass in optional info about your users to help us with targetting. Please
make sure your privacy policy allows this before giving us their personal information.

#### initializeWithApiKey(apiKey)
This method needs to be called before you can use any of the other API calls.
This method initializes your app with the TapForTap so we can begin serving 
you ads.

Usage:

```javascript
// Initialze with TapForTap with my API key
var apiKey = 'My API key from my TapForTap account';
TapForTap.initializeWithApiKey(apiKey);
```

#### createAdView([options])
Create and display an ad view.

The options object itself is optional, and supports these optional properties:

  * **x**: the x coordinate of the origin of the ad (only supported on iOS)
  * **y**: the y coordinate of the origin of the ad (only supported on iOS)
  * **gender**: the user's gender, `'male'` or `'female'`
  * **age**: an integer specifying the user's age
  * **location**: an object with `latitude` and `longitude` properties

Usage:

```javascript
// Create an ad with no options
TapForTap.createAdView();

// Create an ad at the top of the screen (only supported on iOS)
TapForTap.createAdView({ y: 0 });

// Pass in optional info about the user
TapForTap.createAdView({
  gender: 'female',
  age: 21,
  location: { latitude: '123.4567890', longitude: '45.67890' }
});
```

#### loadAds()
Loads and displays Tap for Tap ads. Call this once after creating the ad view.

Usage:

```javascript
// Display the ad view and start loading ads
TapForTap.loadAds();
```

#### moveAdView([options])
This animates the ad view to a new location on screen. `options` is optional
and can contain the following properties:

  * **x**: the x coordinate of the origin of the ad (only supported on iOS)
  * **y**: the y coordinate of the origin of the ad (only supported on iOS)

Usage:

```javascript
// Move the ad to the top of the screen (only supported on iOS)
TapForTap.moveAdView({ y: 0 });
```

#### removeAdView()
Removes the ad view from the screen and stops loading ads.

Usage:

```javascript
// Remove the ad view and stop loading ads
TapForTap.removeAdView();
```

#### prepareInterstitial()
This prepares the interstial ad type. This method only needs to be called once, 
after the interstitial is shown we automatically prepare another one for you.

Usage:

```javascript
// Prepare the insterstial ad type
TapForTap.prepareInsterstitial();
```

#### showInterstitial()
This shows an interstitial ad. Remember that you must call prepareInterstital() once
before calling showInterstitial(). Afterwards you only need to call showInterstial()
to show a new interstitial ad.

Usage:

```javascript
// prepare the interstitial ad type
TapForTap.prepareInterstitial();

// show an interstitial ad
TapForTap.showInterstitial();

// do some neat app stuff
  .
  .
  .

// show another interstital add (Note that we do not need to call
// prepareInterstitial() again)
TapForTap.showInterstital();
```

#### prepareAppWall()
This prepares the app wall ad type. This method only needs to be called once, 
after the app wall is shown we automatically prepare another one for you.

Usage:

```javascript
// Prepare the app wall ad type
TapForTap.prepareAppWall();
```

#### showAppWall()
This shows an app wall ad. Remember that you must call prepareAppWall() once
before calling showAppWall(). Afterwards you only need to call showAppWall()
to show a new app wall ad.

Usage:

```javascript
// prepare the app wall ad type
TapForTap.prepareAppWall();

// show an interstitial ad
TapForTap.showAppWall();

// do some different neat app stuff
  .
  .
  .

// show another app wall add (Note that we do not need to call
// prepareAppWall() again)
TapForTap.showwAppWall();
```

## Support
Things don't always go according to plan. If you hit a snag somewhere and need a
hand don't hesitate to get in touch with us at
[support@tapfortap.com](mailto:support@tapfortap.com) or on Twitter where we go
by the swanky handle [@tapfortap](https://twitter.com/tapfortap).

## License

Licensed under the terms of the MIT License.

Copyright &copy; 2012 Tap for Tap Promotions Inc.
