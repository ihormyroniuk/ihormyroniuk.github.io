# iOS Application Entry Point

Approach to implement an entry point for iOS application is considered in this document.

### By default
By default [Xcode](https://developer.apple.com/xcode/) generates file `AppDelegate.swift`, which contains class `AppDelegate` inherited by [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder), implemented protocol [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) and marked by attribute [`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html). It is proposed that this class is entry point for application. 

As result of this there are two global objects exist at runtime: [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate). [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) is type of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) is type of `AppDelegate`.

### Proposition

Unite these two classes into one.   

### References
[`Swift. Files and Initialization`](https://developer.apple.com/swift/blog/?id=7)

[`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared)

[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate)

[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)
