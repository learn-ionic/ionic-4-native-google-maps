# Ionic 4 Native Google Maps

[![N|Solid](https://www.ning.com/ning3help/wp-content/uploads/2013/07/donate-button.png)](https://paypal.me/MariusBolik)


This simple Project shows you how to implement native Google Maps into a Ionic 3.9.2 App. Contents:

  - App with 3 Tabs
  - Native Google Map
  - A simple Modal

# You need:

  - API Key for Google Maps IOS SDK
  - API Key for Google Maps Android SDK
  *-> For more Info please scroll down*



### Tech

This starter uses a number of open source projects to work properly:

* [AngularJS](https://angularjs.org/) - HTML enhanced for web apps!
* [Ionic](https://ionic.io) - The top open source framework for building amazing mobile apps.
* [Apache Cordova](https://cordova.apache.org/) - Target multiple platforms with one code base.
* [Xcode](https://developer.apple.com/xcode/) - To build iOS Apps
* [Android Studio](https://developer.android.com/studio/index.html) - To build Android Apps
* [Node.js](https://nodejs.org/) - A JavaScript runtime built on Chrome's V8 JavaScript engine

And of course Ionic itself is open source with a [public repository](https://github.com/ionic-team/ionic) on GitHub.

### Installation

After installing dependencies and devDependencie download or clone this repository and run this command:
```sh
$ cd ionic-3-native-google-maps-and-tabs
$ npm install
```
Now install the Google Maps Plugin and insert your API Keys:

```sh
$ ionic cordova plugin add cordova-plugin-googlemaps --variable API_KEY_FOR_ANDROID="GOOGLE_MAPS_KEY_HERE" --variable API_KEY_FOR_IOS="GOOGLE_MAPS_KEY_HERE"
$ npm install --save @ionic-native/google-maps
```

**That's it!** Now test on your Device. Make sure it's connected to your PC.

```sh
$ ionic cordova run ios --device
```

[Not necessary] - Optionally you can build and test it manually:

```sh
$ ionic cordova platform add ios
$ ionic cordova build ios
```

### Get Google Maps API Keys

An account for the [Google Developer](https://console.developers.google.com/) Console is required.
Create a new project:
![Create a new project](https://i.imgur.com/Q5Xu8yU.jpg)

Once this is done, the Google Maps API is required in the Library menu. Choose what you need: iOS or Android.
![Library menu](https://i.imgur.com/RSStYc5.jpg)

And enable the service:
![enable the service](https://i.imgur.com/iY9tgx6.png)

Finally copy the API’s key:
![copy the API’s key](https://i.imgur.com/fbFOOdG.jpg)


# Tutorial

You can ignore this section. This only is for your understanding.
In this litte Tutorial I want to show you how I integrated native Google Maps in an Ionic 3 Project.

### Create Project
First create a new Ionic Project:
```sh
$ ionic start myApp tabs
```

Than start adding the native Google Maps Plugin:
```sh
$ ionic cordova plugin add cordova-plugin-googlemaps --variable API_KEY_FOR_ANDROID="GOOGLE_MAPS_KEY_HERE" --variable API_KEY_FOR_IOS="GOOGLE_MAPS_KEY_HERE"
$ npm install --save @ionic-native/google-maps
```

#### Insert the map to the desired location
The home.html file is our starting point:
```html
<ion-content>
  <div #map style="height:100%;"></div>
</ion-content>
```
#### Editing the Controllers
**Very important** - Open the app.module.ts file to add the Native GoogleMaps Service:
```ts
import { GoogleMaps } from '@ionic-native/google-maps';
.
.
.
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    GoogleMaps
  ]
```
And the imports (ViewChild and Google Maps) in the home.ts file:
```ts
import { Component, ViewChild } from '@angular/core';
import {
 GoogleMaps,
 GoogleMap,
 GoogleMapsEvent,
 LatLng,
 MarkerOptions,
 Marker
} from '@ionic-native/google-maps';
import { Platform, NavController } from 'ionic-angular';
```
Grab the map Element from the DOM and add Google Maps to Constructor:
```ts
export class HomePage {
  @ViewChild('map') element;

  constructor(public googleMaps: GoogleMaps, public plt: Platform, public nav: NavController) {
  }
  .
  .
  .
}
```
Initialize the map view and wait until the map is ready:
```ts
 ngAfterViewInit() {
    this.plt.ready().then(() => {
      this.initMap();
    });
  }
```
When all the lights are green, the following initMap method is triggered:
```typescript
initMap() {
    let map: GoogleMap = this.googleMaps.create(this.element.nativeElement);
    map.one(GoogleMapsEvent.MAP_READY).then((data: any) => {
      let coordinates: LatLng = new LatLng(33.6396965, -84.4304574);
      let position = {
        target: coordinates,
        zoom: 17
      };
      map.animateCamera(position);
      let markerOptions: MarkerOptions = {
        position: coordinates,
        icon: "assets/images/icons8-Marker-64.png",
        title: 'Our first POI'
      };
      const marker = map.addMarker(markerOptions)
        .then((marker: Marker) => {
          marker.showInfoWindow();
      });
    })
  }
```
### Bugfix
If you have a black or white screen showing instead of the map, you might have to add this in the app.scss file:
```css
ion-app .nav-decor{
  background-color: transparent !important;
}
```


### Todos

 - Style the Map
 - Add more functions

License
----

MIT


**Free Software, Hell Yeah!**


