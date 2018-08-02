# Stock Evaluator Mobile

Ionic app that generates Android and iOS apps for **stock-evaluator.com** site.

## Build:
```
ionic build
ionic cordova build android
ionic cordova emulate android
ionic cordova build ios
ionic cordova emulate ios
```

## Sign APK (**janet-release-key.jks** file needed):
```
ionic cordova build android --prod --release

jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore janet-release-key.jks platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk janet

~/Library/Android/sdk/build-tools/28.0.1/zipalign -v 4 platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk platforms/android/app/build/outputs/apk/release/StockEvaluator.apk

~/Library/Android/sdk/build-tools/28.0.1/apksigner verify platforms/android/app/build/outputs/apk/release/StockEvaluator.apk
```

## Deploy
https://ionicframework.com/docs/intro/deploying/

## Common bugs and solutions

### IOS: Resource not found... (8080 port is busy)
https://ionicframework.com/docs/wkwebview/

### Android
https://stackoverflow.com/questions/50935337/failed-to-execute-shell-command-getprop-dev-bootcomplete-on-device-error-for/51005353  
This is a cordova-android bug because Google probably changed the error message when trying to run the app.
It has been fixed already, but not released yet.
I think it would work if you apply this change yourself to platforms/android/cordova/lib/emulator.js
Change
```
if ((error && error.message &&
            (error.message.indexOf('not found') > -1)) ||
            (error.message.indexOf('device offline') > -1))
```
to
```
if ((error && error.message &&
          (error.message.indexOf('not found') > -1)) ||
          (error.message.indexOf('device offline') > -1) ||
          (error.message.indexOf('device still connecting') > -1) ||
          (error.message.indexOf('device still authorizing') > -1))
```

## License

The MIT license. See the bundled LICENSE file for details.
