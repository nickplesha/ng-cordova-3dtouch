ng-cordova-3dtouch
===================

AngularJS wrapper for [Cordova 3D touch plugin](https://github.com/EddyVerbruggen/cordova-plugin-3dtouch). Makes it easy to integrate home screen quick actions handling to your Angular mobile app.

## Install

```
bower install ng-cordova-3dtouch
```

```
cordova plugin add cordova-plugin-3dtouch
```

## Usage

Include `3dtouch.js` after the rest of your Ionic and Angular includes.

```html
<script src="lib/ngCordova-3dtouch/dist/3dtouch.js"></script>
```

Add `ngCordova.plugins.3dtouch` as a module dependency of your app.

## Usage

### Availability

Check if the plugin is loaded and the device supports 3D touch:

```
$cordova3DTouch.isAvailable(function(result) {
    console.log(result); //true or false
});
```

Other functions call this method internally so to simplify your code there is no need to check if 3D touch is available first.


### Home Screen Quick Actions

```
angular.module('example', ['ionic', 'ngCordova.plugins.3dtouch'])

.run(function($ionicPlatform, $cordova3DTouch, $state) {
    $ionicPlatform.ready(function() {

        //Add a dynamic quick action with title: Favorites and a "star" icon
        $cordova3DTouch.addQuickAction('favorites', 'Favorites', 'Favorite' 'Posts you marked as favorite', function() {
            //Navigate to target state when the quick action was pressed on home screen
            $state.go('tab.favorite');
        });
    }

    });
})
```

`Favorite` is the name of one of the default included quick action icons in iOS. You can find a list of them in the [Apple developer documentation.](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/#//apple_ref/c/tdef/UIApplicationShortcutIconType)
Or you can add a custom `.png` icon (size 70x70 or 105x105) to your `Resources` folder in Xcode.

### Static Home Screen Quick Actions

Static quick actions are presents before your app has been opened for the first time. Add them anywhere to your `.plist` file:

```
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeFavorite</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Favorites</string>
        <key>UIApplicationShortcutItemType</key>
        <string>favorites</string>
    </dict>
</array>
```
Then add a handler for your action:

```
angular.module('example', ['ionic', 'ngCordova.plugins.3dtouch'])

.run(function($ionicPlatform, $cordova3DTouch, $state) {
    $ionicPlatform.ready(function() {

        //Add a handler for the static quick action with title: Favorites and a "star" icon
        $cordova3DTouch.addQuickActionHandler('favorites', function() {
            //Navigate to target state when the quick action was pressed on home screen
            $state.go('tab.favorite');
        });
    }

    });
})
```

### Force Touch

You can add a handler function for force touch events. Handler will be called when at least 75% force is applied to the screen. Additional information sent is the timestamp and coordinates.

```
angular.module('example', ['ionic', 'ngCordova.plugins.3dtouch'])

.run(function($ionicPlatform, $cordova3DTouch, $state) {
    $ionicPlatform.ready(function() {

        $cordova3DTouch.addForceTouchHandler(function(touch) {
            console.log("force touch % " + touch.force); // 84
            console.log("force touch timestamp " + touch.timestamp); // 1449908744.706419
            console.log("force touch x coordinate " + touch.x); // 213
            console.log("force touch y coordinate " + touch.y); // 41
        });
    }

    });
})
```

### Link Preview

You can enable link previews in popups when a link is force pressed.

```
angular.module('example', ['ionic', 'ngCordova.plugins.3dtouch'])

.run(function($ionicPlatform, $cordova3DTouch, $state) {
    $ionicPlatform.ready(function() {

        $cordova3DTouch.enableLinkPreview();

    }

    });
})
```


## Used in

ngCordova 3D touch is currently used in Updraft for Reddit (www.upvote.io)

## License

MIT
