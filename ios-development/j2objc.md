# Objective-C

github, google/j2objc: [https://github.com/google/j2objc](https://github.com/google/j2objc)

google/j2objc: [https://developers.google.com/j2objc/](https://developers.google.com/j2objc/)

J2ObjC is an open-source command-line tool from Google that translates Java source code to Objective-C for the iOS \(iPhone/iPad\) platform. The goal is to write an app's non-UI code \(such as business logic and **data models**\) in Java, which is then shared by web apps \(using [GWT](http://www.gwtproject.org/)\), Android apps, and iOS apps.

{% hint style="info" %}
GWT is a development toolkit for building and optimizing complex **browser-based** applications.
{% endhint %}

## KVC and KVO

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

Key-value observing \(KVO\) provides a mechanism that allows objects to be notified of changes to specific properties of other objects.

