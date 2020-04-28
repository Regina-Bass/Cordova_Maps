# Cordova_Maps
Cordova Maps Application for browser, IOS and android


### 1: Create Cordova Project
`cordova create GMapExample com.labs.gmapexample GMapExample`
### 2: Change Working Directory
`cd GMapExample`
### 3: Add the Geolocation Plugin
`cordova plugin add cordova-plugin-geolocation`
The documentation for this plugin can be found at: https://github.com/apache/cordova-plugin-geolocation

### 4: Google Map JavaScript Library
Don't add this yet, the complete code for this app is in step 6.

Just note that this is the script tag that should be added to the index.html file for apps that use the Google Maps plugin, because the plugin uses the Google Maps API.

`<script src="http://maps.google.com/maps/api/js"></script>`
### 5: Get User’s Location Using Geolocation Plugin
Don't add this yet, just read it. The complete code for this app is in step 6.

This code is presented here to show you exactly how the getCurrentPosition() function works in the plugin. There are other methods in this plugin and we will look at those later. The getCurrentPosition() method will get the current position if the location services are turned on in your device. Then the position is passed to the onSuccess() method. In the complete app, the longitude and latitude values are used to create the map we want to see on our screen.

 ` navigator.geolocation.getCurrentPosition( onSuccess, onError, { timeout: 30000 } ); `

`   function onSuccess( position ) {
      let lat = position.coords.latitude,
          lng=position.coords.longitude;
   } `

 `  function onError(error) {
      alert('code: ' + error.code + '\n' + 'message: ' + error.message + '\n');
   } 
 `

### 6. Full Code for the App
Replace the index.html file in the app you created above with the code below:

`<!doctype html>`
 <html lang="en-us">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Geo Location</title>
    <style>
      html { height: 100% }
      body { height: 100%; margin: 0; padding: 0 }
      #map-canvas { height: 100% }
    </style>
    <script src="cordova.js"></script>
    <script src="http://maps.google.com/maps/api/js"></script>
    <script>
      navigator.geolocation.getCurrentPosition( onSuccess, onError, { timeout: 30000 } );`
 
      function onSuccess( position ) {
         if ( position.coords ) {
            let lat = position.coords.latitude,
              lng=position.coords.longitude,
 
            //Google Maps
              myLatlng = new google.maps.LatLng( lat, lng ),
              mapOptions = { zoom: 3, center: myLatlng },
              map = new google.maps.Map( document.getElementById( 'map-canvas' ), mapOptions ),
              marker = new google.maps.Marker( { position: myLatlng, map: map } );
         }
      }
 
      function onError(error) {
         alert('code: ' + error.code + '\n' + 'message: ' + error.message + '\n');
      }
 
      google.maps.event.addDomListener( window, 'load', onSuccess );
    </script>
 </head>
 <body>
    <div id="map-canvas"><h3>Loading map. Please wait...</h3></div>
  </body>
 </html> `

### 7. Add the Platform(s)
`cordova platform add browser`
### 8. Run the App
`cordova run browser`
Or open the platforms → browser → www → index.html file in your web browser.

Try running the iOS and Android apps too.

### 9. iOS
There's a quirk for iOS; you have to specify the "NSLocationWhenInUseUsageDescription", which describes the reason that the app accesses the user's location.

Add the following inside the root element in config.xml:

`<edit-config target="NSLocationWhenInUseUsageDescription" file="*-Info.plist" mode="merge">
   <string>need location access to find things nearby</string>
</edit-config>`
Now add the platform

`cordova platform add ios`
and open platforms → ios → GMapExample.xcodeproj with Xcode. You should be able to run the app.

### 10. Android
On the IST iMacs:
In order to specify an Android version that's compatible with the geolocation plugin, use:

`cordova platform add android@8.0.0`
Then import the project into Android Studio (be sure to hit cancel for any update dialogs).

On your own computer:
`cordova platform add android`
You should be able to run the app.

If the geolocation service times out, in the Android emulator go to Settings → Security & Location → Location (under Privacy) → Mode, and enable "High accuracy". You can force stop the app by going to Settings → Apps & notifications → App info → GMapExample → Force Stop. Then reopen the app in the emulator, cross your fingers, and you should see the map appear.

You can also change "your location" by clicking the "More" button in the emulator's toolbar and adjusting the latitude and longitude. The location is only read when the app starts, so you'll have to restart the app in order to read in the new location.

### 11. Experiment
Once you've got it working, try changing the options for the map, such as making it zoom in more, hiding controls, or changing to satellite view.

You'll have to rebuild the platform(s) after you make changes:

`cordova platform update browser`
