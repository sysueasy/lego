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

## Objective-C Runtime

The Objective-C Runtime **module** APIs define the **base** of the Objective-C language. These APIs include:

* Types such as the **NSObject** class and the **NSObject** **protocol** that provide the root functionality of most Objective-C classes.
* Functions and data structures that comprise the Objective-C runtime, which provides support for the dynamic properties of the Objective-C language.

You typically don't need to use the Objective-C runtime library directly when programming in Objective-C. This API is useful primarily for developing bridge layers between Objective-C and other languages, or for low-level debugging.

**NSObject** is the root class of most Objective-C class hierarchies, from which subclasses inherit a basic interface to the runtime system and the ability to behave as Objective-C objects.



