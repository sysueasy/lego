# Functions

C programs generally consist of many small functions rather than a few big ones. A program may reside in one or more **source files**. Source files may be compiled separately and loaded together, along with previously compiled functions from **libraries**.

## External Variables

Each **local** variable in a function comes into existence only when the function is called, and disappears when the function is exited, such variables are known as **automatic variables**.

It is possible to define variables that are **external** to all functions, that is, variables that can be accessed by name by any function.

An external variable must be **defined**, exactly once, outside of any function; this sets aside storage for it. The variable must also be **declared** in each function that wants to access it; this states the type of the variable.

If an external variable is to be referred to before it is defined, or if it is defined in a different source file from the one where it is being used, then an **extern** declaration is mandatory.

If the definition of the external variable occurs in the source file before its use in a particular function, then there is no need for an **extern** declaration in the function.

The usual practice is to collect extern declarations of variables and functions in a separate file, called a **header**, that is included by `#include` at the front of each source file.

Relying too heavily on external variables is fraught with peril since it leads to programs whose data connections are not all obvious - variables can be changed in unexpected and even inadvertent ways, and the program is hard to modify.

## Scope

The **scope** of a name is the part of the program within which the name can be used.

For an **automatic variable** declared at the beginning of a function, the scope is the function in which the name is declared.

The scope of an **external variable** or a **function** lasts from the point at which it is declared to the end of the file being compiled. 

## Static

The **static** declaration, applied to an external variable or function, limits the scope of that object to the rest of the **source file** being compiled.

Internal static variables are local to a particular function just as automatic variables are, but unlike automatics, they remain in existence rather than coming and going each time the function is activated.

## Register

A register declaration advises the compiler that the variable in question will be heavily used. The idea is that register variables are to be placed in machine registers, which may result in smaller and faster programs. But compilers are free to ignore the advice.

The register declaration can only be applied to automatic variables and to the formal parameters of a function.

## Initialization

For **external** and **static** variables, the initializer must be a constant expression; the initialization is done once, conceptionally before the program begins execution.

For automatic and register variables, it may be any expression involving previously defined values, even function calls.

An array may be initialized: `int steps[] = { 1, 2 };`

Character arrays are a special case of initialization: `char pattern[] = "ould";`is a shorthand for the longer but equivalent `char pattern[] = { 'o', 'u', 'l', 'd', '\0' };`

## Recursion

A function may call itself either directly or indirectly. When a function calls itself recursively, each invocation gets a fresh set of all the automatic variables, independent of the previous set.

{% hint style="info" %}
A good example of recursion is quicksort. Given an array, one element is chosen and the others partitioned in two subsets. The same process is then applied recursively to the two subsets. When a subset has fewer than two elements, it doesn't need any sorting; this stops the recursion.
{% endhint %}

Recursion may provide no saving in storage, since somewhere a stack of the values being processed must be maintained. Nor will it be faster. But recursive code is more compact, and often much easier to write and understand than the non-recursive equivalent. Recursion is especially convenient for recursively defined data structures like trees.

## The C Preprocessor

C provides certain language facilities by means of a preprocessor, which is conceptionally a separate first step in compilation. The two most frequently used features are `#include`, and `#define`. Others include conditional compilation and macros 宏命令 with arguments.

Any source line of the form `#include`is replaced by the **contents** of the file _filename_. If the filename is quoted, **searching** for the file typically begins where the source program was found; if it is not found there, or if the name is enclosed in &lt; and &gt;, searching follows an implementation-defined rule to find the file.

`#define name replacement text`

It calls for a **macro** substitution of the simplest kind - subsequent occurrences of the token _name_ will be replaced by the _replacement text_.

Any name may be **defined** with any replacement text. For example, defines a new word, forever, for an infinite loop: `#define forever for (;;)`

It is also possible to define macros with arguments: `define max(A, B) ((A) > (B) ? (A) : (B))`

Names may be **undefined** with `#undef`, usually to ensure that a routine is really a function, not a macro.

It is possible to control preprocessing itself with **conditional** statements that are evaluated during preprocessing. This provides a way to include code selectively.

The `#if` line evaluates a constant integer expression. If the expression is non-zero, subsequent lines until an `#endif` or `#elif` or `#else` are included. For example, to make sure that the contents of a file are **included only once**:

```c
#if !defined(HDR)
#define HDR
/* contents of hdr.h go here */
#endif
```

The `#ifdef` and `#ifndef` lines are specialized forms that test whether a **name** is defined. The example above could have been written:

```c
#ifndef HDR
#define HDR
/* contents of hdr.h go here */
#endif
```

If this style is used consistently, then each header can itself include any other headers on which it depends, without the user of the header having to deal with the interdependence.

