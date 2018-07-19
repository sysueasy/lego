# The C Programming Language

The first program to write is the same for all languages:

```c
#include <stdio.h> // include information about standard input/output library
// define a function called main
int main(int argc, char const *argv[]) //arguments
{ // statement enclosed in braces
    printf("hello, world\n"); 
    return 0;
}
```

On the UNIX operating system you must create the program in a file such as `hello.c`, then compile it with the command `cc hello.c`, make an executable file called `a.out`.

To run, type command `a.out`, in macOS, use `./a.out`. It will print these words.

A C program, whatever its size, consists of **functions** and **variables**. A function contains **statements** that specify the computing operations to be done, and variables store values used during the computation. 

Our example is a function named `main`. Normally you are at liberty to give functions whatever names you like, but `main` is special - your program begins executing at the beginning of main. This means that every program must have a main somewhere.

A sequence of characters in double quotes, like "hello, world\n", is called a **character string** or **string constant**.

An **escape sequence** like \n provides a general and extensible mechanism for representing hard-to-type or invisible characters.

C provides **data types** including: `int, float, char, short, long, double`. The size of these objects depends on the machine you are using \(32-bit or 64-bit\). There are also `arrays, structures, unions` of these basic types, `pointers` to them, and `functions` that return them.

The while loop operates as follows: The condition in parentheses is tested. If it is true the body of the loop is executed. Then the condition is re-tested, and if true, the body is executed again.

```c
while (fahr <= upper) { }
```

Although C compilers do not care about how a program looks, proper _indentation_ and _spacing_ are critical in making programs easy for people to read. We recommend writing only one statement per line, and using _blanks around operators_ to clarify grouping.

In C, as in many other languages, **integer** **division** _**truncates**_: any fractional part is discarded, e.g 5/9 would be truncated to zero.

`%6.2f` : print as floating point, at least 6 wide and 2 after decimal point. `printf` also recognizes %o for octal, %x for hexadecimal, %c for character, %s for character string and %% for itself.



