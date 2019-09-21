# iOS Application Entry Point

### In General

iOS application is a [Swift](https://swift.org/) application. It means it has same entry point - [`main.swift`](https://developer.apple.com/swift/blog/?id=7) file. Then control is transfered to [`UIKit`](https://developer.apple.com/documentation/uikit) framework, using function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain), which sets up the main event loop, including the application’s run loop, and begins processing events. Also it instantiates the application object [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared), its delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) (if any) and sets the delegate for the application.

In result there are two global objects at runtime: [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate). [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) is type of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)
and
[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) is type of [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate). 

### [Xcode](https://developer.apple.com/xcode/)

By default [Xcode](https://developer.apple.com/xcode/) generates file `AppDelegate.swift`, which contains class `AppDelegate` inherited by [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder), implemented protocol [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) and marked by attribute [`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html). Applying this attribute to a class to indicate that it’s the application delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate). Using this attribute is equivalent to calling the function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain) indicating object of attributed class as delegate for the application object [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared). It is proposed to consider class `AppDelegate` as entry point for application. In particular, methods [`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application) and [`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application).

### Problems

There are several problems with this approach:
1. There are 2 global objects: [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate).
2. Sometimes it is needed to have ability to override [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) methods.

### Objective

Describe approach to implement an generalized entry point for iOS application to solve above problems.

### Proposition

Unite [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) objects into one. In other words, make application object its own delegate. To accomplish that, it is needed to make subclass of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) and set it as  application object's class.

### Implementation

1. Delete `AppDelegate.swift` file (or at least delete attribute [`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html) for class `AppDelegate`).
2. Create `Application.swift` file with class `Application` is subclass of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication), which implements [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate).

``` swift
import UIKit

class Application: UIApplication, UIApplicationDelegate {
  
}
```

3. Create [`main.swift`](https://developer.apple.com/swift/blog/?id=7) file, where call function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain), specifying class `Application` in parameters `principalClassName` and `delegateClassName`.

``` swift
import UIKit

UIApplicationMain(
    CommandLine.argc,
    CommandLine.unsafeArgv, 
    NSStringFromClass(Application.self),
    NSStringFromClass(Application.self)
)
```

### Notes

1. You can declare reference to application object in [`main.swift`](https://developer.apple.com/swift/blog/?id=7) file, after calling function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain).

``` swift
import UIKit

...

var application: Application! {
  return UIApplication.shared as? Application
}
```
2. You can use [`AUIApplication`](https://github.com/ihormyroniuk/AUIKit/blob/master/AUIKit/Application/AUIApplication.swift) protocol and its implementations ([`AUIEmptyApplication`](https://github.com/ihormyroniuk/AUIKit/blob/master/AUIKit/Application/AUIEmptyApplication.swift)) to inherit your `Application` class.

### In Result

There is one global object at runtime. It is [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) of type `Application`, which allows us to override [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) methods.

### References
[Swift. Files and Initialization](https://developer.apple.com/swift/blog/?id=7)\
[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)\
[`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html)\
[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)\
[`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared)\
[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)\
[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate)\
[How to subclass UIApplication using UIApplicationMain](https://www.hackingwithswift.com/example-code/uikit/how-to-subclass-uiapplication-using-uiapplicationmain)
