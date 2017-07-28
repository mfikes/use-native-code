# use-native-code

Attempts to use some native code following the CalendarManager exaple at https://facebook.github.io/react-native/docs/native-modules-ios.html

The problem is that the `RCT_EXPORT_MODULE();` code runs and registers the native code, but 

```
(.-NativeModules (js/require "react-native"))
```

yields `#js {}`

To repro, clone this repo, run `yarn` and then `react-native run-ios` and `re-natal use-figwheel` and `lein figwheel ios`. Then try evaluating the form above that looks at the `NativeModules` property.
