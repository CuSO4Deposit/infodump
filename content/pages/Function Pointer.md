---
tags:
- C
- Pointer
date: 2021-09-20
title: Function Pointer
categories:
lastMod: 2025-10-18
--- 
## Definition

A function pointer is a pointer to a function. Here's an example showing function pointer declaration and function call.

```c
#include <stdio.h>
// A normal function with an int parameter
// and void return type
void fun(int a){
  printf("Value of a is %d\n", a);
}

int main(){
  // fun_ptr is a pointer to function fun()
  void (*fun_ptr)(int) = &fun;

  /* The above line is equivalent of following two
  void (*fun_ptr)(int);
  fun_ptr = &fun;
  */

  // Invoking fun() using fun_ptr
  (*fun_ptr)(10);

  return 0;
}
```

and the example's output:

Value of a is 10
  
## Properties

1. Unlike normal pointers, a function pointer points to code, not data. Typically a function pointer stores the start of executable code.
2. Unlike normal pointers, we do not allocate de-allocate memory using function pointers.
3. A function’s name can also be used to get functions’ address. For example, in the below program, we have removed address operator ‘&’ in assignment. We have also changed function call by removing *, the program still works.

```c
#include <stdio.h>
// A normal function with an int parameter
// and void return type
void fun(int a){
  printf("Value of a is %d\n", a);
}

int main(){
  void (*fun_ptr)(int) = fun; // & removed

  fun_ptr(10); // * removed

  return 0;
}
```

Output:

Value of a is 10

4. Like normal pointers, we can have an array of function pointers. Below example in point 5 shows syntax for array of pointers.
5. Function pointer can be used in place of switch case. For example, in below program, user is asked for a choice between 0 and 2 to do different tasks.

```c
#include <stdio.h>
void add(int a, int b){
  printf("Addition is %d\n", a+b);
}
void subtract(int a, int b){
  printf("Subtraction is %d\n", a-b);
}
void multiply(int a, int b){
  printf("Multiplication is %d\n", a*b);
}

int main(){
  // fun_ptr_arr is an array of function pointers
  void (*fun_ptr_arr[])(int, int) = {add, subtract, multiply};
  unsigned int ch, a = 15, b = 10;

  printf("Enter Choice: 0 for add, 1 for subtract and 2 "
          "for multiply\n");
  scanf("%d", &ch);

  if (ch > 2) return 0;

  (*fun_ptr_arr[ch])(a, b);

  return 0;
}
```

Enter Choice: 0 for add, 1 for subtract and 2 for multiply
2
Multiplication is 150 

6. Many object oriented features in C++ are implemented using function pointers in C. For example [virtual functions](https://www.geeksforgeeks.org/virtual-functions-and-runtime-polymorphism-in-c-set-1-introduction/). Class methods are another example implemented using function pointers. Refer [this book](http://www.cs.rit.edu/~ats/books/ooc.pdf) for more details.
  
## Reference

1. [Function Pointer in C](https://www.geeksforgeeks.org/function-pointer-in-c/)

2. [Secure Function Pointer and Callbacks in Windows Programming](https://www.codeproject.com/articles/191299/secure-function-pointer-and-callbacks-in-windows-p)

3. http://www.cs.cmu.edu/~ab/15-123S11/AnnotatedNotes/Lecture14.pdf
 