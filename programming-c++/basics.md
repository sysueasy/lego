# Basics

## Variables and Basic Types

Some languages, such as Smalltalk and Python, check types at run time. In contrast, C++ is a statically typed language; type checking is done at compile time.

The arithmetic types are divided into two categories: **integral** types \(which include character and boolean types\) and **floating**-point types. The size of—that is, the number of bits in—the arithmetic types varies across machines. Because the number of bits varies, the largest \(or smallest\) value that a type can represent also varies.

The smallest chunk of addressable memory is referred to as a “**byte**.” Most computers associate a number \(called an “**address**”\) with each byte in memory.

A value, such as 42, is known as a **literal** because its value self-evident. Every literal has a type. The form and value of a literal determine its type.

Each **variable** in C++ has a type. The type determines the size and layout of the variable’s memory. C++ programmers tend to refer to variables as “variables” or “objects” interchangeably.

To support separate **compilation**, C++ distinguishes between declarations and definitions. To use a variable in more than one file requires **declarations** that are separate from the variable’s **definition**. Variables must be defined exactly once but can be declared many times.

Variable names are visible from the point where they are declared until the end of the scope in which the declaration appears. The name defined outside any curly braces has **global** scope.

When we define a **reference**, instead of copying the initializer’s value, we bind the reference to its initializer. Once initialized, a reference remains bound to its initial object. There is no way to rebind a reference to refer to a different object. Because there is no way to rebind a reference, references must be initialized.

```cpp
int ival = 1024;
int &refVal = ival;
```

A reference is an alias. A reference is not an object. Instead, a reference is just another name for an already existing object. Because references are not objects, they don’t have addresses.

Unlike a reference, a **pointer** is an object in its own right. Pointers can be assigned and copied; a single pointer can point to several different objects over its lifetime. Unlike a reference, a pointer need not be initialized at the time it is defined.

A pointer holds the address of another object. We get the address of an object by using the address-of operator \(the **&** operator\). When a pointer points to an object, we can use the dereference operator \(the **\*** operator\) to access that object.

```cpp
int ival = 42;
int *p = &ival;
std::cout << *p;
```

Two pointers are equal if they hold the same address and unequal otherwise.

The type `void*` is a special pointer type that can hold the address of any object. There are only a limited number of things we can do with a void pointer: We can compare it to another pointer, we can pass it to or return it from a function, and we can assign it to another void pointer.

{% hint style="info" %}
It can be easier to understand complicated pointer or reference declarations if you read them from right to left.
{% endhint %}

We can make a variable unchangeable by defining the variable’s type as **const**. Because we can’t change the value of a const object after we create it, it must be initialized.

To define a single instance of a const variable, we use the keyword **extern** on both its definition and declaration\(s\):

```cpp
// file_1.cc defines and initializes a const that is accessible to other files
extern const int bufSize = fcn();
// file_1.h
extern const int bufSize; // same bufSize as defined in file_1.cc
```

A **constant expression** is an expression whose value cannot change and that can be evaluated at compile time. A literal is a constant expression.

