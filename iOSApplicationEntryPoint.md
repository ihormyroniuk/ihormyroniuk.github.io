# iOS Application Entry Point

Approach to implement an entry point for iOS application is considered in this document.

### By default
By default [Xcode](https://developer.apple.com/xcode/) generates file `AppDelegate.swift`, which contains class `AppDelegate` inherited by [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder), implemented protocol [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) and marked by attribute [`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html).

As result of this there are two global objects at runtime: [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate). [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) is type of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)
and
[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) is type of `AppDelegate`. It is proposed that class `AppDelegate` is entry point for application.

### Proposition

Unite these two objects into one. In other words, make application object own delegate. To accomplish that, it is needed to make subclass of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) and set it as  application object's class.

### How To

1. Delete `AppDelegate.swift` file
2. Create subclass of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication), which implements [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
3. Create [`main.swift`](https://developer.apple.com/swift/blog/?id=7) file

``` swift
import UIKit

UIApplicationMain(
    CommandLine.argc,
    CommandLine.unsafeArgv, 
    NSStringFromClass(Application.self),
    NSStringFromClass(Application.self)
)
```

### References
[`Swift. Files and Initialization`](https://developer.apple.com/swift/blog/?id=7)\
[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)\
[`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html)\
[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)\
[`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared)\
[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)\
[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate)\
[How to subclass UIApplication using UIApplicationMain](https://www.hackingwithswift.com/example-code/uikit/how-to-subclass-uiapplication-using-uiapplicationmain)
