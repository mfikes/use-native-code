# use-native-code

Attempts to use some native code following the CalendarManager example at https://facebook.github.io/react-native/docs/native-modules-ios.html

The problem is that the `RCT_EXPORT_MODULE();` code runs and registers the native code, but 

```
(.-NativeModules (js/require "react-native"))
```

yields `#js {}`

This repo was created using `re-natal init UseNativeCode` followed by adding the `CalendarManager.m` and `.h` files.

To repro, clone this repo, run `yarn` and then `react-native run-ios` and `re-natal use-figwheel` and `lein figwheel ios`. Then try evaluating the form above that looks at the `NativeModules` property.

If you `re-natal xcode` and add a breakpoint on the line with `RCT_EXPORT_MODULE();` in `CalendarManager.m`, you'll see that the native code module is registered.

If you enable remote JS debugging, then things will work (and when doing so you can see that when setting up the bridge with Chrome, the `RCT_EXPORT_MODULE();` macro in `CalendarManager` gets invoked again.

```
ios:cljs.user=> (js-keys (.-NativeModules (js/require "react-native")))
#js ["CalendarManager" "StatusBarManager" "SourceCode" "AlertManager" "ExceptionsManager" "DevMenu" "AsyncLocalStorage" "DevSettings" "ScrollViewManager" "JSCSamplingProfiler" "AccessibilityManager" "DevLoadingView" "Timing" "AppState" "DeviceInfo" "Clipboard" "PlatformConstants" "KeyboardObserver" "I18nManager" "WebViewManager" "RedBox" "UIManager" "NavigatorManager" "NativeAnimatedModule" "ActionSheetManager" "LocationObserver" "ImageStoreManager" "ImageViewManager" "ImageEditingManager" "LinkingManager" "NetInfo" "Networking" "SettingsManager" "Vibration" "WebSocketModule"]
```

Update: (Credit to Paulus Esterhazy)

`NativeModules` appears to be a magical object for which you cannot enumerate the keys, but things still work:

```
ios:cljs.user=> (.. (js/require "react-native") -NativeModules -CalendarManager)
#js {:addEvent #object[fn "function fn() {
      for (var _len3 = arguments.length, args = Array(_len3), _key3 = 0; _key3 < _len3; _key3++) {
        args[_key3] = arguments[_key3];
      }

      var lastArg = args.length > 0 ? args[args.length - 1] : null;
      var secondLastArg = args.length > 1 ? args[args.length - 2] : null;
      var hasSuccessCallback = typeof lastArg === 'function';
      var hasErrorCallback = typeof secondLastArg === 'function';
      hasErrorCallback && invariant(hasSuccessCallback, 'Cannot have a non-function arg after a function arg.');
      var onSuccess = hasSuccessCallback ? lastArg : null;
      var onFail = hasErrorCallback ? secondLastArg : null;
      var callbackCount = hasSuccessCallback + hasErrorCallback;
      args = args.slice(0, args.length - callbackCount);
      BatchedBridge.enqueueNativeCall(moduleID, methodID, args, onFail, onSuccess);
    }"]}
```
