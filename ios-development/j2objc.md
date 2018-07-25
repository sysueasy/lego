# Objective-C

## J2ObjC

github, google/j2objc: [https://github.com/google/j2objc](https://github.com/google/j2objc)

google/j2objc: [https://developers.google.com/j2objc/](https://developers.google.com/j2objc/)

J2ObjC is an open-source command-line tool from Google that translates Java source code to Objective-C for the iOS platform. The goal is to write an app's non-UI code \(such as **business logic** and **data models**\) in Java, which is then shared by web apps \(using [GWT](http://www.gwtproject.org/)\), Android apps, and iOS apps.

{% hint style="info" %}
GWT \(Google Web Toolkit\) is a development toolkit for building and optimizing complex **browser-based** applications.
{% endhint %}

See [How Google Inbox shares 70% of its code across Android, iOS, and the Web](https://arstechnica.com/information-technology/2014/11/how-google-inbox-shares-70-of-its-code-across-android-ios-and-the-web/). Google has built itself a good enough arsenal of cross compilers that it can write an app's logic once for Android in Java, and can then cross-compile to Objective-C for iOS and JavaScript for browsers. Java-to-JavaScript is handled by the **GWT**.

## KVO and KVC

Key-value observing \(KVO\) provides a mechanism that allows objects to be notified of changes to specific properties of other objects.

When an object is key-value coding \(KVC\) compliant, its properties are addressable via string parameters through a concise, uniform messaging interface.

```objectivec
@interface BankAccount : NSObject
@property (nonatomic) NSNumber* currentBalance;              // An attribute
@property (nonatomic) Person* owner;                         // A to-one relation
@property (nonatomic) NSArray< Transaction* >* transactions; // A to-many relation
@end
```

Because the `BankAccount` class is key-value coding compliant, it recognizes the keys `owner`, `currentBalance`, and `transactions`, which are the names of its properties. Instead of calling the `setCurrentBalance:` method, you can set the value by its key:

`[myAccount setValue:@(100.0) forKey:@"currentBalance"];`.

## Runtime

The [Objective-C Runtime](https://developer.apple.com/documentation/objectivec?language=objc) **module** APIs define the **base** of the Objective-C language. These APIs include:

* Types such as the **NSObject** class and the **NSObject** **protocol** that provide the root functionality of most Objective-C classes.
* Functions and data structures that comprise the Objective-C runtime, which provides support for the dynamic properties of the Objective-C language.

You typically don't need to use the Objective-C runtime library directly when programming in Objective-C. This API is useful primarily for developing bridge layers between Objective-C and other languages, or for low-level debugging.

**NSObject** is the root class of most Objective-C class hierarchies, from which subclasses inherit a basic interface to the runtime system and the ability to behave as Objective-C objects.

## NSAutoreleasePool

[NSAutoreleasePool](https://developer.apple.com/documentation/foundation/nsautoreleasepool?language=objc) is an object that supports Cocoa’s reference-counted memory management system.

In a **reference-counted** environment \(as opposed to one which uses **garbage collection**\), an `NSAutoreleasePool` object contains objects that have received an `autorelease` message and when drained it sends a `release` message to each of those objects. Thus, sending `autorelease` instead of `release` to an object **extends the lifetime** of that object at least until the pool itself is drained \(it may be longer if the object is subsequently retained\). An object can be put into the same pool several times, in which case it receives a `release` message for each time it was put into the pool.

```objectivec
int main(int argc, char * argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

In a reference counted environment, Cocoa expects there to be an autorelease pool **always available**. If a pool is not available, autoreleased objects do not get released and you leak memory. In this situation, your program will typically log suitable warning messages.

The Application Kit creates an autorelease pool on the **main thread** at the beginning of **every cycle** **of the event loop**, and **drains it at the end**, thereby releasing any autoreleased objects generated while processing an event. If you use the Application Kit, you therefore typically don’t have to create your own pools. If your application creates _a lot of temporary autoreleased objects within the event loop_, however, it may be beneficial to create “local” autorelease pools to help to minimize the peak memory footprint.

```objectivec
@autoreleasepool {
    // Code benefitting from a local autorelease pool.
}
```

If you use Automatic Reference Counting \(ARC\), you cannot use autorelease pools directly.`@autoreleasepool` blocks are more efficient than using an instance of `NSAutoreleasePool` directly; you can also use them even if you do not use ARC.

Each thread \(including the main thread\) maintains its own **stack** of `NSAutoreleasePool`objects. As new pools are created, they get added to the **top of the stack**. When pools are deallocated, they are removed from the stack. Autoreleased objects are placed into the **top autorelease pool** for the current thread. When a thread terminates, it automatically drains all of the autorelease pools associated with itself.

### Threads

If you are making Cocoa calls outside of the Application Kit’s main thread—for example if you create a Foundation-only application or if you detach a thread—you need to create your own autorelease pool.

If your application or thread is long-lived and potentially generates a lot of autoreleased objects, you should **periodically drain and create** autorelease pools \(like the Application Kit does on the main thread\); otherwise, autoreleased objects accumulate and your memory footprint grows. If, however, your detached thread does not make Cocoa calls, you do not need to create an autorelease pool.

### Garbage Collection

In a garbage-collected environment, there is no need for autorelease pools. You may, however, write a framework that is designed to work in both a garbage-collected and reference-counted environment. In this case, you can use autorelease pools to **hint** to the collector that collection may be appropriate.

In a garbage-collected environment, sending a `drain` message to a pool triggers garbage collection if necessary; `release`, however, is a no-op.

In a reference-counted environment, `drain` has the same effect as `release`. Typically, therefore, you should use `drain` instead of `release`.

