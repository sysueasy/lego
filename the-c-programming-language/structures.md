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

Don't assume that the size of a structure is the sum of the sizes of its members. Because of alignment requirements for different objects, there may be unnamed "holes" in a structure. The following structure might well require eight bytes, not five.

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





