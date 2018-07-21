# The C Programming Language

## Hello World

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

