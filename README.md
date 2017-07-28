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
