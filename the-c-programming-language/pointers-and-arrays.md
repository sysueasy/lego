# Pointers and Arrays

A **pointer** is a variable that contains the **address** of a variable.

A typical machine has an array of consecutively numbered or addressed **memory** cells that may be manipulated individually or in contiguous groups. One common situation is that any **byte** can be a _char_, a pair of one-byte cells can be treated as a _short_ integer, and four adjacent bytes form a _long_.

A pointer is a group of cells \(often two or four\) that can hold an address. So if _c_ is a _char_ and _p_ is a _pointer_ that points to it, we could represent the situation this way:

![](../.gitbook/assets/screen-shot-2018-07-21-at-20.38.05.png)

The unary operator **&** gives the address of an object, so the statement `p = &c;` assigns the address of _c_ to the variable _p_, and _p_ is said to "**point to**" _c_.

The & operator only applies to objects in memory: variables and array elements. It cannot be applied to expressions, constants, or _register_ variables.

The unary operator **\*** is the _indirection_ or _dereferencing_ operator; when applied to a pointer, it accesses the object the pointer points to.

```c
int x = 1, y = 2, z[10];
int *ip, *iq; /* ip is a pointer to int */
ip = &x; /* gives the address of x to ip, ip now points to x */
y = *ip; /* access the object, y is now 1 */
*ip = 0; /* x is now 0 */
ip = &z[0]; /* ip is now points to z[0] */
iq = ip; /* iq is now points to whatever ip pointed to */
```

Since C passes arguments to functions by value, there is no direct way for the called function to alter a variable in the calling function.

For instance, a sorting routine might exchange two out-of-order arguments with a function called _swap_. It is not enough to write `void swap(int x, int y)` because of **call by value**, _swap_ can't affect the arguments.

The way to obtain the desired effect is for the calling program to pass pointers to the values to be changed: `swap(&a, &b);`Since the operator & produces the address of a variable, `&a` is a pointer to a. In swap itself, the parameters are declared as pointers, and the operands are accessed indirectly through them: `void swap(int *px, int *py) { }`

 In C, there is a strong relationship between **pointers and arrays**. Any operation that can be achieved by array subscripting can also be done with pointers.

```c
int *pa; /* pa is a pointer points to int */
pa = &a[0]; /* now pa contains the address of a[0]. */
```

If _pa_ points to a particular element of an array, then by definition _pa+1_ points to the **next** element.

![](../.gitbook/assets/screen-shot-2018-07-21-at-21.30.50.png)

These remarks are **true** regardless of the type or size of the variables in the array. The meaning of "adding 1 to a pointer," and by extension, all pointer arithmetic, is that _**pa+1**_ **points to the next object**, and is that _pa+i_ points to the i-th object beyond pa.

Since the name of an array is a synonym for the location of the initial element, the assignment `pa=&a[0]` can also be written as `pa = a`.

Rather more surprising, at first sight, is the fact that a reference to `a[i]` can also be written as `*(a+i)`. In evaluating `a[i]`, C converts it to `*(a+i)` immediately, the two forms are equivalent. As the other side of this coin, if _pa_ is a pointer, expressions might use it with a subscript; `pa[i]` is identical to `*(pa+i)`. 

In short, an array-and-index expression is **equivalent** to one written as a pointer and offset.

There is one **difference** between an array name and a pointer that must be kept in mind. A pointer is a variable, but an array name is not a variable; constructions like `a=pa` and `a++` are illegal.

When an array name is passed to a function, what is passed is the **location** of the initial element.

As formal parameters in a function definition, `char s[];` and `char *s;`are equivalent. we prefer the **latter** because it says more explicitly that the variable is a pointer.

```c
/* strlen: return length of string s */
    int strlen(char *s){
    int n;
    for (n = 0; *s != '\0', s++)
        n++;
    return n;
}
```

Since s is a pointer, incrementing it is perfectly legal; `s++` has no effect on the character string in the function that called _strlen_, but merely increments strlen's **private copy of the pointer**.



