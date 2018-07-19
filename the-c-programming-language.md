# The C Programming Language

The first program to write is the same for all languages:

```c
#include <stdio.h> // include information about standard input/output library
// define a function called main
int main(int argc, char const *argv[]) //arguments参数
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

`%6.2f` : print as floating point, at least 6 wide and 2 after decimal point.

A `#define` line defines a **symbolic name** or **symbolic constant** to be a particular string of characters, conventionally written in **upper case** so they can be readily distinguished from lower case variable names.

```c
#include <stdio.h>
int main(){ /* copy input to output; 1st version */
    int c;
    while ((c = getchar()) != EOF)
        putchar(c);
}
```

`EOF` is an integer defined in `<stdio.h>`as a symbolic constant, its value is **-1**.

A assignment can appear as part of a larger expression.

You could instead write `c = c + 1` but `++c` is more concise and often more efficient.

By definition, `char` are just small integers. A character written between single quotes represents an **integer** value in the machine's character set, called a **character constant**. `'A'` is a character constant; in the ASCII character set its value is 65.

Since `main` is a **function** like any other, a return value of zero implies normal termination; non-zero values signal unusual or erroneous termination conditions.

In C, all function **arguments** are passed \`\`by value.'' This means that the called function is given the values of its arguments in temporary variables rather than the originals.





