# iOS Application Entry Point

### Overview

iOS application is a [Swift](https://swift.org/) application. It means it has entry point same as all [Swift](https://swift.org/)'s applications - [`main.swift`](https://developer.apple.com/swift/blog/?id=7) file. Then control is transfered to [`UIKit`](https://developer.apple.com/documentation/uikit) framework, using function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain), which sets up the main event loop, including the application’s run loop, and begins processing events. Also it instantiates the application object [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared), its delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) (if any) and sets the delegate for the application. 
The application object [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) is the centralized point of control and coordination for applications running in iOS. [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) delegate has methods [`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application) and [`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application). Use these methods to initialize your app and prepare it to run.

### [Xcode](https://developer.apple.com/xcode/)

By default [Xcode](https://developer.apple.com/xcode/) generates file `AppDelegate.swift`, which contains class `AppDelegate` inherited by [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder), implemented protocol [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) and marked by attribute [`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html). Applying this attribute to a class it indicates that it’s the application delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate). Using this attribute is equivalent to calling the function [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain) indicating object of attributed class as delegate for the application object [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared). It is proposed to consider class `AppDelegate` as entry point for application, in particular, methods [`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application) and [`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application).  
**In my opinion**, it is not optimal approach to implement iOS application entry point, because there are two objects [`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared) and its delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate), which are responsible for applications running. It is more explicit to unite these objects into one. In other words, make application object its own delegate. To accomplish that, it is needed to make subclass of [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication), implement protocol [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) for it, set it its own delegate [`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate) and use it as application object's class.  
**!!!IMPORTANT!!!** You can read more about subclassing [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) [here](https://developer.apple.com/documentation/uikit/uiapplication) and designating the subclass as the delegate [here](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain). This approach gives more control for application running, **so that's good**.

#### Implementation

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

#### Notes

1. You can use [`AUIApplicationMain`](https://github.com/ihormyroniuk/AUIKit/blob/master/AUIKit/AUIApplicationMain.swift) function instead of [`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain).
2. You can use [`AUIApplication`](https://github.com/ihormyroniuk/AUIKit/blob/master/AUIKit/Application/AUIApplication.swift) protocol and its implementations ([`AUIEmptyApplication`](https://github.com/ihormyroniuk/AUIKit/blob/master/AUIKit/Application/AUIEmptyApplication.swift)) to inherit your `Application` class.

### References

[Swift. Files and Initialization](https://developer.apple.com/swift/blog/?id=7)  
[`UIApplicationMain`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)  
[`@UIApplicationMain`](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html)  
[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)  
[`UIApplication.shared`](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared)  
[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)  
[`UIApplication.shared.delegate`](https://developer.apple.com/documentation/uikit/uiapplication/1622936-delegate)  
[`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)  
[`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)