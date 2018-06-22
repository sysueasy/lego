# 2. Object-oriented Programming

Real-world objects share two characteristics: They all have **state** and **behavior**.

For each object that you see, ask yourself two questions: "What possible states can this object be in?" and "What possible behavior can this object perform?".

Software objects are conceptually similar to real-world objects: they too consist of state and related behavior. An object stores its state in **fields** \(**variables** in some programming languages\) and exposes its behavior through **methods** \(**functions** in some programming languages\). Hiding internal state and requiring all interaction to be performed through an object's methods is known as **data encapsulation**.

A **class** is the blueprint from which individual objects are created. An **interface** is a group of related methods with empty bodies. A class can implement more than one interface.

```java
class MyClass extends MySuperClass implements YourInterface {
    // field, constructor, and
    // method declarations
}
```

A **final** method cannot be overridden in a subclass. Methods called from constructors should generally be declared final. A class that is declared **final** cannot be subclassed. This is particularly useful, for example, when creating an immutable class like the String class.

A **package** is a **namespace** that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer. 

The Java programming language defines the following kinds of variables:

* Instance Variables
* Class Variables: this tells the compiler that there is exactly **one** copy of this variable in existence, regardless of how many times the class has been instantiated.
* Local Variables
* Parameters

## Garbage collection

An object is eligible for **garbage collection** when there are no more references to that object. References that are held in a variable are usually dropped when the variable goes out of scope. Or, you can explicitly drop an object reference by setting the variable to the special value **null**. The Java runtime environment has a garbage collector that **periodically** frees the memory used by objects that are no longer referenced.

## Access Control

Two levels of access control:

At the class level—**public**, or _package-private_ \(no explicit modifier\).

At the member level—**public**, **private**, **protected**, or _package-private_ \(no explicit modifier\). 

\*The protected modifier specifies that the member can only be accessed within its own package \(as with package-private\) and, in addition, by a subclass of its class in another package.

## Nested Classes

Nested class is a way of logically **grouping** classes that are only used in only one other class. Nested classes that are declared static are called **static nested classes**. Non-static nested classes are called **inner classes**.

Java doesn't allow you to create top-level static classes; only nested \(inner\) static classes. The static inner class acts like an _entirely separate class_. We don't need an instance of the outer class to create an object of a static inner class. The only exception to static classes acting like completely separate classes to their enclosing classes, is that static inner classes can access static data members of the enclosing class -- or call static methods, for that matter.

A nested class is a **member** of its enclosing class. Non-static nested classes \(inner classes\) have access to other members of the enclosing class, even if they are declared private. There are two additional types of inner classes. You can declare an inner class within the body of a method. These classes are known as **local classes**. You can also declare an inner class within the body of a method without naming the class. These classes are known as **anonymous classes**.

If a declaration of a type in a particular scope has the same name as another declaration in the enclosing scope, then the declaration **shadows** the declaration of the enclosing scope. You cannot refer to a shadowed declaration by its name alone:

```java
// shadowing
public class ShadowTest {
    public int x = 0;
    class FirstLevel {
        public int x = 1;
        void methodInFirstLevel(int x) {
            // ...
        }
    }
    // x = 23
    //this.x = 1
    //ShadowTest.this.x = 0
}
```

## Annotations

The predefined annotation types defined in _java.lang_ are **@Deprecated**, **@Override**, and **@SuppressWarnings**.

Annotation types are a form of interface:

```java
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   String[] reviewers();
}
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   reviewers = {"Alice", "Bob", "Cindy"}
)
```

Annotations that apply to other annotations are called **meta-annotations**.

## Interfaces

There are situations when it is important for disparate groups of programmers to agree to a "contract" that spells out how their software interacts. Each group should be able to write their code without any knowledge of how the other group's code is written. Generally speaking, interfaces are such contracts.

For example, automobile manufacturers may write software that operates the automobile—stop, start, accelerate, and so forth.

```java
public interface OperateCar {
   // constant declarations, if any
   // method signatures
   boolean stop();
   boolean start();
}
```

The other parties can then write software that **invokes** the methods described in the interface to command the car. Neither group needs to know how the other group's software is implemented. This example shows an interface being used as an industry standard _Application Programming Interface_ \(API\).

In Java, an interface is a **reference type**. Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces.

The interface body contains **abstract methods**. An abstract method within an interface is followed by a semicolon, but no braces \(an abstract method does not contain an implementation\). All methods in an interface are implicitly public, so you can omit the public modifier.

An interface can contain **constant** declarations. All constant values defined in an interface are implicitly public, static, and final.

A class that implements an interface must implement **all** the methods declared in the interface. In case of adding new methods to an interface, users who have classes that implement interfaces enhanced with new **default** or **static** methods do not have to modify or recompile them to accommodate the additional methods.

## Polymorphism

```java
public class TestBikes {
  public static void main(String[] args){
    Bicycle bike01, bike02, bike03;

    bike01 = new Bicycle(20, 10, 1);
    bike02 = new MountainBike(20, 10, 5, "Dual");
    bike03 = new RoadBike(40, 20, 8, 23);

    bike01.printDescription();
    bike02.printDescription();
    bike03.printDescription();
  }
}
```

Polymorphism: Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class - in this case, by using `super.printDescription();`

The Java virtual machine \(JVM\) calls the appropriate method for the object that is referred to in each variable. It does not call the method that is defined by the variable's type.

## Abstract Classes

An abstract method is a method that is declared without an implementation \(without braces, and followed by a semicolon\). An **abstract class** is a class that is declared abstract—it may or may not include abstract methods. If a class includes abstract methods, then the class itself must be declared abstract.

Abstract classes cannot be instantiated, but they can be subclassed. When an abstract class is subclassed, the subclass usually provides implementations for all of the abstract methods in its parent class.

{% hint style="info" %}
Methods in an interface \(see the Interfaces section\) that are not declared as default or static are _implicitly_ abstract, so the abstract modifier is not used with interface methods.
{% endhint %}

Abstract classes are similar to interfaces. However, with abstract classes, you can declare fields that are not static and final, and define public, protected, and private **concrete** methods. In addition, you can extend only one class, whether or not it is abstract, whereas you can implement any number of interfaces.

Consider using abstract classes if:

* You want to share code among several closely related classes. 
* You expect that classes that extend your abstract class have many common methods or fields.
* You want to declare non-static or non-final fields. 

Consider using interfaces if:

* You expect that unrelated classes would implement your interface.
* You want to specify the behavior of a particular data type, but not concerned about who implements its behavior.
* You want to take advantage of multiple inheritance of type.

An example of a class in the JDK use both abstract classes and interfaces; the **HashMap** class implements several interfaces and also extends the abstract class AbstractMap.

