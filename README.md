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
