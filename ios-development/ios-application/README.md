# Documentation

## UIKit

The [UIKit](https://developer.apple.com/documentation/uikit) framework provides the required infrastructure for your **iOS** or **tvOS** apps. It provides the window and view architecture for implementing your interface, the event handling infrastructure for delivering Multi-Touch and other types of input to your app, and the main [run loop](./#runloop) needed to manage interactions among the user, the system, and your app.

The [structure](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit) of UIKit apps is based on the **Model-View-Controller** \(MVC\) design pattern. You provide the model objects that represent your app’s data structures. UIKit provides most of the view objects. Coordinating the exchange of data between your data objects and the UIKit views are your view controllers and app delegate object.

![](../../.gitbook/assets/ff7aa08f-4857-44ce-88d5-7dacbef84509.png)

UIKit provides most of the objects in the controller and view layers of your app. Specifically, UIKit defines the [`UIView`](https://developer.apple.com/documentation/uikit/uiview) class, which is usually responsible for displaying your content onscreen. \(You can also render content directly to the screen using [Metal](https://developer.apple.com/documentation/metal) and other system frameworks.\) The [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) object runs your app’s main event loop and manages your app’s overall life cycle.

## UIKit - Core App

[UIKit apps](https://developer.apple.com/documentation/uikit/core_app) are always in one of five states:

![](../../.gitbook/assets/high_level_flow_2x_2bc77269-019d-4554-83b8-6aeecb73c348.png)

Apps start off **not running**. When the user explicitly launches the app, the app moves briefly to the **inactive** state before entering the **active** state. \(An active app appears onscreen and is known as a **foreground** app.\) Quitting an active app moves it offscreen and into the _**background**_ state, where it stays until the system **suspends** it a short time later. At its discretion, the system may quietly _**terminate**_ a suspended app, returning it to the not running state.

`applicationWillResignActive(_:)` called when your app is about to move from the active to inactive state. This can occur for certain types of temporary interruptions \(such as an incoming **phone call** or **SMS message**\) or when the user quits the app and it begins the transition to the background state. An app in the inactive state continues to run but does not dispatch incoming events to responders.

`applicationDidEnterBackground(_:)`Use this method to release shared resources, invalidate timers, and store enough app state information to restore your app to its current state in case it is terminated later. Your implementation of this method has approximately **five seconds** to perform any tasks and return.

### Launch

![](../../.gitbook/assets/76e68c08-6b09-4bac-8a00-44df7a097a43.png)

1. The app is launched, either explicitly by the user or implicitly by the system.
2. The Xcode-provided `main` function calls UIKit's `UIApplicationMain(_:_:_:_:)`function.
3. The `UIApplicationMain(_:_:_:_:)` function creates the `UIApplication` object and your app delegate. 
4. UIKit loads your app's default interface from the main storyboard or nib file.
5. UIKit calls your app delegate's `application(_:willFinishLaunchingWithOptions:)` method.
6. UIKit performs state restoration, which calls additional methods of your app delegate and view controllers.
7. UIKit calls your app delegate's `application(_:didFinishLaunchingWithOptions:)` method.

### UIApplication

Every iOS app has exactly one instance of UIApplication \(or, very rarely, a subclass of UIApplication\). A major role of your app’s application object is to handle the initial routing of incoming user events\(e.g. touches\). The application object maintains a list of open windows \(UIWindow objects\) and through those can retrieve any of the app’s UIView objects.

### Universal Links

While [universal links](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content) and custom URLs are both acceptable forms of deep linking, _universal links_ _are strongly recommended as a **best practice**_. Key **benefits** are \(1\) One URL works for both your website and your app, allowing your website to handle the link when your app is not installed. \(2\) iOS verifies the association through the Apple App Site Association file on your website, eliminating the possibility that other apps might claim your scheme and redirect your URLs.

{% hint style="danger" %}
Universal links offer a potential **attack** vector into your app, so make sure to validate all URL parameters and discard any malformed URLs.
{% endhint %}

UIKit apps can communicate through universal links. Supporting universal links allows **other apps** to send small amounts of data directly to your app without using a third-party server. Define the parameters that your app handles within the URL **query** string.

## UIKit - Interactions

Apps receive and **handle events** using _responder objects_. A responder object is any instance of the `UIResponder` class, and common subclasses include `UIView`, `UIViewController`, `UIWindow`, and `UIApplication`. Responders receive the raw event data \([`UIEvent`](https://developer.apple.com/documentation/uikit/uievent)\) and must either handle the event or forward it to another responder object.

When your app receives an event, UIKit automatically directs that event to the most appropriate responder object, known as the **first responder**. Unhandled events are passed from responder to responder in the active **responder chain**, which is the dynamic configuration of your app’s responder objects.

You can alter the responder chain by overriding the `next` property of your responder objects. Many UIKit classes already override this property and return specific objects.

* UIView: If the view is the root view of a view controller, the next responder is the view controller.
* UIViewController: If the view controller’s view is the root view of a window, the next responder is the window object. If the view controller was presented by another view controller, the next responder is the presenting view controller.
* UIWindow: The window's next responder is the UIApplication object.
* UIApplication: The next responder is the app delegate.

![](../../.gitbook/assets/f17df5bc-d80b-4e17-81cf-4277b1e0f6e4.png)

UIKit designates an object as the first responder to an event based on the **type** of that event. There are several kinds of events:

* Touch events are the most common and are delivered to the view in which the touch originally occurred.
* Motion events are UIKit triggered and are separate from the motion events reported by the Core Motion framework. \(e.g shake to undo\)
* Remote-control events allow a responder object to receive commands from an external accessory or headset.
* Press events represent interactions with a game controller, AppleTV remote, or other device that has physical buttons.

To handle a specific type of event, a responder must **override** the corresponding methods. For example, to handle touch events, a responder implements the [`touchesBegan(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621142-touchesbegan), etc. methods.

**Gesture** recognizers receive touch and press events before their view does.

UIKit compares the **touch** location to the bounds of view objects in the view hierarchy. The [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method of [`UIView`](https://developer.apple.com/documentation/uikit/uiview) traverses the view hierarchy, looking for the **deepest** subview that contains the specified touch, which becomes the first responder for the touch event. If a touch location is outside of a view’s bounds, the `hitTest(_:with:)` method ignores that view and all of its subviews.

## Foundation

The [Foundation](https://developer.apple.com/documentation/foundation) framework provides a base layer of functionality for apps and frameworks, including:

* text processing, date and time calculations, sorting and filtering, The [Swift Standard Library](https://developer.apple.com/documentation/swift/swift_standard_library) provides many of the same types available in the Foundation framework.
* data storage and persistence
* networking, e.g. [URL Loading System](url-loading-system.md).
* XPC, Object Runtime, [Process and Threads](operation.md) \(e.g. RunLoop,  Operation\), 

The classes, protocols, and data types defined by Foundation are used throughout the macOS, iOS, watchOS, and tvOS SDKs.

