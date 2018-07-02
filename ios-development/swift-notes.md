# Swift

## Value and Reference Type

Ref: [https://developer.apple.com/swift/blog/?id=10](https://developer.apple.com/swift/blog/?id=10)

Types in Swift fall into one of two categories: first, “**value types**”, where each instance keeps a unique copy of its data, usually defined as a `struct`, `enum`, or tuple. The second, “**reference types**”, where instances share a single copy of the data, and the type is usually defined as a class.

## In-Out Parameters

Function parameters are constants by default. If you want a function to modify a parameter’s value, and you want those changes to persist after the function call has ended, define that parameter as an in-out parameter instead.

You place an ampersand \(&\) directly before a variable’s name when you pass it as an argument to an in-out parameter, to indicate that it can be modified by the function: `swapTwoInts(&someInt, &anotherInt)`

## Enumerations

An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.

If you are familiar with C, you will know that C enumerations assign related names to a set of **integer** values. Enumerations in Swift are much more flexible, and do not have to provide a value for each case of the enumeration. If a value \(known as a “**raw**” value\) is provided for each enumeration case, the value can be a string, a character, or a value of any integer or floating-point type.

**Associated value**: “Define an enumeration type called Barcode, which can take either a value of upc with an associated value of type \(Int, Int, Int, Int\), or a value of qrCode with an associated value of type String.”

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

## Methods

Structures and enumerations are [value types](swift-notes.md#value-and-reference-type). By default, the properties of a value type cannot be modified from within its instance methods.

However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to **mutating** behavior for that method.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
```

## Initialization

Unlike Objective-C initializers, Swift initializers do not return a value.

```swift
init() { 
// perform some initialization here
}
```

When you assign a default value to a stored property, or set its initial value within an initializer, the value of that property is set directly, without calling any **property observers**. 

**Structure** types automatically receive a memberwise initializer if they do not define any of their own custom initializers. Unlike a default initializer, the structure receives a memberwise initializer even if it has stored properties that do not have default values.

```swift
struct Size { 
    var width: Double
    var height: Double 
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

Swift provides a **default initializer** for any structure or class that provides default values for all of its properties and does not provide at least one initializer itself. The default initializer simply creates a new instance with all of its properties set to their default values.

```swift
class ShoppingListItem { 
    var name: String? 
    var quantity = 1 
    var purchased = false 
} 
var item = ShoppingListItem()
```

Swift defines two kinds of initializers for **class** types to help ensure all stored properties receive an initial value. These are known as designated initializers and convenience initializers.

A **designated** initializer fully initializes all properties introduced by that class and calls an appropriate superclass initializer to continue the initialization process up the **superclass chain**. Classes tend to have very few designated initializers, and it is quite common for a class to have only one.

Initializers can call other initializers to perform part of an instance’s initialization. This process, known as initializer delegation, avoids duplicating code across multiple initializers. **Convenience** initializers are secondary initializers that call a designated initializer from the same class.

```swift
class Food {
    var name: String 
    init(name: String) { //designated
        self.name = name 
    } 
    convenience init() { //convenience
        self.init(name: "[Unnamed]") 
    } 
}
```

Class initialization in Swift is a two-phase process. In the first phase, each stored property is assigned an initial value by the class that introduced it. The second phase is the opportunity to customize its stored properties further \(optional\).

Swift’s **two-phase initialization** is similar Objective-C. The main difference is that during phase 1, Objective-C assigns zero or null values to every property. Swift’s initialization flow is more flexible in that it lets you set custom initial values.

Swift performs four helpful safety-checks to make sure that two-phase initialization is completed, otherwise you will get _compile error_:

* A designated initializer must ensure that all of the properties introduced by its class are initialized before it delegates up to a superclass initializer.
* A designated initializer must delegate up to a superclass initializer before assigning a value to an inherited property.
* A convenience initializer must delegate to another initializer before assigning a value to any property.
* An initializer cannot call any instance methods, read the values of any instance properties, or refer to self as a value until after the first phase of initialization is complete.

![](../.gitbook/assets/initializerdelegation01_2x.png)

Once the top of the superclass chain is reached, and the final class in the chain has ensured that all of its stored properties have a value, the instance’s memory is considered to be fully initialized, and phase 1 is complete. Then in phase 2, working back down from the top of the chain, each designated initializer in the chain has the option to customize the instance further. Initializers are now able to access self and can modify its properties, call its instance methods, and so on.

When you write a subclass initializer that matches a superclass designated initializer, you are effectively providing an **override** of that **designated** initializer. This is true even if you are overriding an automatically provided default initializer.

Conversely, if you write a subclass initializer that matches a superclass convenience initializer, that superclass convenience initializer can never be called directly by your subclass. As a result, you do not write the override modifier.

```swift
class RecipeIngredient: Food { 
    var quantity: Int 
    init(name: String, quantity: Int) { 
        self.quantity = quantity //new property introduced in subclass
        super.init(name: name) //delegates up
        self.name = self.name + "Ingredient" //assigning new value to an inherited property.
    } 
    override convenience init(name: String) { 
        self.init(name: name, quantity: 1) 
    }
}
```

Unlike subclasses in Objective-C, Swift subclasses do not **inherit** their superclass initializers by default. However, if you provide default values for any new properties you introduce in a subclass, then:

* If your subclass doesn’t define any designated initializers, it automatically inherits all of its superclass designated initializers.
* If your subclass provides an implementation of all of its superclass designated initializers—either by inheriting them as per rule 1, or by providing a custom implementation—then it automatically inherits all of the superclass convenience initializers.

Write the **required** modifier before the definition of a class initializer to indicate that every subclass of the class must implement that initializer. You do not have to provide an explicit implementation of a required initializer if you can satisfy the requirement with an inherited initializer.

## Protocol

A _protocol_ defines a blueprint of **methods**, **properties**, and other requirements that suit a particular task or piece of functionality. The protocol can then be _adopted_ by a **class**, **structure**, or **enumeration** to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to _conform_ to that protocol.

In addition to specifying requirements that conforming types must implement, you can extend a protocol to implement some of these requirements or to implement additional functionality that conforming types can take advantage of.

**Delegation** is a design pattern that enables a class or structure to hand off \(or _delegate_\) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type \(known as a delegate\) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.

To prevent strong reference cycles, delegates are declared as **weak** references.

```swift
weak var delegate: DiceGameDelegate?
```

## Auto Reference Counting

In most cases, memory management “just works” in Swift. Every time you create a new instance of a class, ARC allocates a chunk of memory to store information about that instance. When an instance is no longer needed, ARC frees up the memory used by that instance so that the memory can be used for other purposes instead.

Whenever you assign a class instance to a property, constant, or variable, that makes a strong reference to the instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists.

If two class instances hold a strong reference to each other, such that each instance keeps the other alive. This is known as a **strong reference cycle**. You resolve it by defining some of the relationships between classes as **weak** or **unowned** references instead of as strong references.

A weak reference does not keep a strong hold on the instance it refers to, and so does not stop ARC from disposing of the referenced instance. ARC automatically sets a weak reference to nil when the instance that it refers to is deallocated. So they are always declared as variables, rather than constants, of an **optional** type.

An unowned reference is expected to always have a value. As a result, ARC never sets an unowned reference’s value to nil, which means that unowned references are defined using **nonoptional** types.

Use an unowned reference only when you are sure that the reference always refers to an instance that has not been deallocated. If you try to access the value of an unowned reference after that instance has been deallocated, you’ll get a runtime error.

{% hint style="info" %}
Compared with system that use garbage collection, with ARC, values are deallocated as soon as their last strong reference is removed.
{% endhint %}

A situation where two properties, both of which are allowed to be nil, is best resolved with a weak reference.

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
}
```

A situation where one property that is allowed to be nil and another property that cannot be nil is best resolved with an unowned reference.

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
}
```

However, there is a third scenario, in which both properties should always have a value, and neither property should ever be nil once initialization is complete. In this scenario, it’s useful to combine an unowned property on one class with an implicitly unwrapped optional property on the other class.

```swift
class Country {
    let name: String
    var capitalCity: City! //default nil
    init(name: String, capitalName: String) {
    //as described in Two-Phase Initialization
        self.name = name //phase 1 completed
        //in phase 2, can start to reference the implicit self
        self.capitalCity = City(name: capitalName, country: self) 
    }
}

class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}
```

A strong reference cycle can also occur if you assign a **closure** to a property of a class instance, and the body of that closure captures the instance. You resolve a strong reference cycle between a closure and a class instance by defining a **capture list** as part of the closure’s definition.

```swift
lazy var someClosure: (Int, String) -> String = {
    [unowned self, weak delegate = self.delegate!] (index: Int, stringToProcess: String) -> String in
    // closure body goes here
}
```

Define a capture in a closure as an **unowned** reference when the closure and the instance it captures will always refer to each other, and will always be deallocated at the same time. Conversely, define a capture as a **weak** reference when the captured reference may become nil at some point in the future.

## Memory Safety

A conflicting access to memory can occur when different parts of your code are trying to access the same location in memory at the same time. Multiple accesses to a location in memory at the same time can produce unpredictable or inconsistent behavior.

The conflicting access discussed here can even happen on a single thread and doesn’t involve concurrent or multithreaded code. For example:

```swift
var stepSize = 1
func increment(_ number: inout Int) {
    number += stepSize
}
increment(&stepSize)
```

Specifically, a conflict occurs if you have two accesses that meet all of the following conditions: At least one is a write access. They access the same location in memory. Their durations overlap.

An access is _instantaneous_ if it’s not possible for other code to run after that access starts but before it ends. Most memory access is instantaneous. However, there are several ways to access memory, called _long-term_ accesses, that span the execution of other code.

A function has long-term write access to all of its [in-out parameters](swift-notes.md#in-out-parameters). 

A [mutating](swift-notes.md#methods) method on a structure has write access to self for the duration of the method call.

[Value types](swift-notes.md#value-and-reference-type) are made up of individual constituent values, mutating any piece of the value mutates the whole value, meaning read or write access to one of the properties requires read or write access to the whole value.

If the compiler can’t prove the access is safe, it doesn’t allow the access.

