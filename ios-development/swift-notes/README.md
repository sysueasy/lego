# Swift

## Implicitly Unwrapped Optionals

Sometimes it’s clear from a program’s structure that an optional will always have a value, after that value is first set. In these cases, it’s useful to remove the need to check and unwrap the optional’s value every time it’s accessed. 

For example: `var capitalCity: City!`means that the `capitalCity` property has a default value of nil, like any other optional, but can be accessed without the need to unwrap its value.

Don’t use an implicitly unwrapped optional when there’s a possibility of a variable becoming nil at a later point.

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

## Initialization

Unlike Objective-C initializers, Swift initializers do not return a value. When you assign a default value to a stored property, or set its initial value within an initializer, the value of that property is set directly, without calling any **property observers**. 

```swift
init() { 
// perform some initialization here
}
```

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

Initializers can call other initializers to perform part of an instance’s initialization. This process, known as initializer delegation, avoids duplicating code across multiple initializers. **Convenience** initializers are secondary initializers that call a designated initializer from the same class with some of the parameters set to default values.

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

Swift applies the following three rules for delegation calls between initializers:

* A designated initializer must call a designated initializer from its immediate superclass.
* A convenience initializer must call another initializer from the same class.
* A convenience initializer must ultimately call a designated initializer.

![](../../.gitbook/assets/initializerdelegation01_2x.png)

Class initialization in Swift is a two-phase process. In the first phase, each stored property is assigned an initial value by the class that introduced it. The second phase is the opportunity to customize its stored properties further \(optional\).

Swift’s **two-phase initialization** is similar Objective-C. The main difference is that during phase 1, Objective-C assigns zero or null values to every property. Swift’s initialization flow is more flexible in that it lets you set custom initial values.

Swift performs four helpful safety-checks to make sure that two-phase initialization is completed, otherwise you will get _compile error_:

* A designated initializer must ensure that all of the properties introduced by its class are initialized before it delegates up to a superclass initializer.
* A designated initializer must delegate up to a superclass initializer before assigning a value to an inherited property.
* A convenience initializer must delegate to another initializer before assigning a value to any property.
* An initializer cannot call any instance methods, read the values of any instance properties, or refer to self as a value until after the first phase of initialization is complete.

Once the top of the superclass chain is reached, and the final class in the chain has ensured that all of its stored properties have a value, the instance’s memory is considered to be fully initialized, and phase 1 is complete. Then in phase 2, working back down from the top of the chain, each designated initializer in the chain has the option to customize the instance further. Initializers are now able to access self and can modify its properties, call its instance methods, and so on.

When you write a subclass initializer that matches a superclass _designated_ initializer, you are effectively providing an **override** of that designated initializer. This is true even if you are overriding an automatically provided default initializer.

Conversely, if you write a subclass initializer that matches a superclass _convenience_ initializer, that superclass convenience initializer can never be called directly by your subclass. As a result, you do not write the override modifier.

```swift
class RecipeIngredient: Food { 
    var quantity: Int 
    init(name: String, quantity: Int) { 
        self.quantity = quantity //new property introduced in subclass
        super.init(name: name) //delegates up
        self.name = self.name + "Ingredient" //assigning new value to an inherited property.
    } 
    /* You always write the override modifier 
    when overriding a superclass designated initializer, 
    even if your subclass’s implementation of the initializer 
    is a convenience initializer.
    */
    override convenience init(name: String) { 
        self.init(name: name, quantity: 1) 
    }
}
```

Unlike subclasses in Objective-C, Swift subclasses do not inherit their superclass initializers by default. However, if you provide default values for any new properties you introduce in a subclass, then:

* If your subclass doesn’t define any designated initializers, it automatically inherits all of its superclass designated initializers.
* If your subclass provides an implementation of all of its superclass designated initializers—either by inheriting them as per rule 1, or by providing a custom implementation—then it automatically inherits all of the superclass convenience initializers.

Write the **required** modifier before the definition of a class initializer to indicate that every subclass of the class must implement that initializer:

```swift
class SomeClass {
    required init() {
        // initializer implementation goes here
    }
}
```

You do not have to provide an explicit implementation of a required initializer if you can satisfy the requirement with an inherited initializer.

## Auto Reference Counting

In most cases, this means that memory management “just works” in Swift. Every time you create a new instance of a class, ARC allocates a chunk of memory to store information about that instance. When an instance is no longer needed, ARC frees up the memory used by that instance so that the memory can be used for other purposes instead.

Whenever you assign a class instance to a property, constant, or variable, that makes a strong reference to the instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists.

If two class instances hold a strong reference to each other, such that each instance keeps the other alive. This is known as a **strong reference cycle**. You resolve it by defining some of the relationships between classes as **weak** or **unowned** references instead of as strong references.

A weak reference does not keep a strong hold on the instance it refers to, and so does not stop ARC from disposing of the referenced instance. ARC automatically sets a weak reference to nil when the instance that it refers to is deallocated. So they are always declared as variables, rather than constants, of an **optional** type.

An unowned reference is expected to always have a value. As a result, ARC never sets an unowned reference’s value to nil, which means that unowned references are defined using **nonoptional** types.

Use an unowned reference only when you are sure that the reference always refers to an instance that has not been deallocated. If you try to access the value of an unowned reference after that instance has been deallocated, you’ll get a runtime error.

{% hint style="info" %}
Compared with system that use garbage collection, with ARC, values are deallocated as soon as their last strong reference is removed.
{% endhint %}



