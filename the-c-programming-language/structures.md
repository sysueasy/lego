# Structures

A structure is a collection of one or more variables, possibly of different types, grouped together under a single name for convenient handling.

Structures help to organize complicated data, particularly in large programs, because they permit a group of related variables to be treated as a unit instead of as separate entities.

```c
struct point { // structure tag
    int x; // member
    int y; 
};

int main(int argc, char const *argv[]) {
    struct point pt; // declaration
    struct point maxpt = { 320, 200 }; // initialization
    printf("%d,%d", pt.x, pt.y); //member operator
    return 0;
}

struct rect { // nested structure
    struct point pt1;
    struct point pt2;
};
```

The only legal operations on a structure are copying it or assigning to it as a unit, taking its address with &, and accessing its members.

If a large structure is to be passed to a function, it is generally more efficient to pass a **pointer** than to copy the whole structure. Structure pointers are just like pointers to ordinary variables.

```c
/* origin is a structure of type struct point */
struct point origin, *pp;
pp = &origin; /* pp is a pointer, it now points to origin */
/* (*pp).x and (*pp).y are the members */
```

Pointers to structures are so frequently used that an alternative **notation** is provided as a shorthand.

```c
printf("origin is (%d, %d)\n", pp->x, pp->y);
```

The structure operators `.` and `->`, together with `()` for function calls and `[]` for subscripts, are at the **top** of the precedence hierarchy and thus bind very tightly.

```c
struct {
    int len;
    char *str;
} *p;
// ++p->len increments len, not p. ++(p->len)
// *p->str fetches whatever str points to; *(p->str)
```

##  Arrays of Structures

Consider writing a program to count the occurrences of each C keyword.

```c
struct key {
    char *word;
    int count;
} keytab[] = {
    "auto", 0,
    "break", 0,
    "case", 0,
    /* ... */
    "void", 0,
    "while", 0
};
```

Here inner braces like `{ "auto", 0 }` are not necessary when the initializers are simple variables or character strings, and when all are present.

Don't assume that the size of a structure is the sum of the sizes of its members. Because of **alignment requirements** for different objects, there may be unnamed "holes" in a structure. The following structure might well require eight bytes, not five.

```c
struct {
    char c; // a char is one byte
    int i; // an int is four bytes
};
```

C provides a compile-time unary operator called `sizeof` that can be used to compute the size of any object.

The expressions `sizeof object` yield an integer equal to the size of the specified object or type **in bytes**. Strictly, `sizeof` produces an unsigned integer value whose type, `size_t`, is defined in the header _&lt;stdio.h&gt;_.

Thus, the number of keywords is the size of the array which can be defined by:

```c
#define NKEYS (sizeof keytab / sizeof(struct key))
```

## Data Structures

To represent a node in binary search tree in C:

```c
struct tnode { /* the tree node */
    char *word;
    int count;
    struct tnode *left; // left child
    struct tnode *right; // right child
};
```

A **storage allocator** is needed to make a new tnode, where we make use of the standard library function _malloc_, we will discuss this in detail in later chapters.

```c
#include <stdlib.h>
/* talloc:  make a tnode */
struct tnode *talloc(void) {
    return (struct tnode *) malloc(sizeof(struct tnode));
}
```

## Typedef

C provides a facility called _typedef_ for creating new data type names. 

```c
typedef char *String;
```

The declaration makes _String_ a synonym for char \* or character pointer, which may then be used in declarations and casts:

```c
String p, lineptr[MAXLINES], alloc(int);
```

As a more complicated example, we could make _typedefs_ for the tree nodes shown earlier in this chapter:

```c
typedef struct tnode *Treeptr;
typedef struct tnode { /* the tree node: */
    // ...
} Treenode;
```

This creates two new type keywords called _Treenode_ \(a structure\) and _Treeptr_ \(a pointer to the structure\).

Besides purely aesthetic issues, there are two main reasons for using _typedef_s.

The first is to parameterize a program against portability problems. If _typedef_s are used for data types that may be machine-dependent, only the _typedefs_ need change when the program is moved. One common situation is to use _typedef_ names for various integer quantities, then make an appropriate set of choices of short, int, and long for each host machine. Types like _size\_t_ and _ptrdiff\_t_ from the standard library are examples.

The second purpose of _typedef_s is to provide better documentation for a program - a type called _Treeptr_ may be easier to understand than one declared only as a pointer to a complicated structure.

## Unions

A union is a variable that may hold \(at different times\) objects of different types and sizes, with the compiler keeping track of size and alignment requirements.

```c
union u_tag {
    int ival;
    float fval;
    char *sval;
} u;
```

The variable _u_ will be large enough to hold the largest of the three types; the specific size is implementation-dependent. Any of these types may be assigned to _u_ and then used in expressions.

Syntactically, members of a union are accessed as: `u->ival`

It is the programmer's responsibility to keep track of which type is currently stored in a union. If the variable _utype_ is used to keep track of the current type stored in _u_, then one might see code such as

```c
if (utype == INT)
   printf("%d\n", u.ival);
if (utype == FLOAT)
   printf("%f\n", u.fval);
if (utype == STRING)
   printf("%s\n", u.sval);
else
   printf("bad type %d in utype\n", utype);
```

