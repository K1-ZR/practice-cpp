# Memory allocation, Stack, Heap

## Intro
**Scope:** where in a program a variable can be referenced.

**Lifetime:** how long a variable exist in memory.  

**Local variables:** variables that are declared within a function or block of code. 
*block of code can be loop, condition, ... .*  
*Their scope* is limited from the point of declaration to the end of function or block of code in which they are declared.  
*Their lifetime* is from entering the function or block of code to termination of function or block of code.  

**Global variables:** variables declared outside of any function.  
*Their scope* is anywhere in the program.  
*Their lifetime* is the duration of the program.  

## Stack
C++ uses the following memory allocation: 

| **heap (free store)**  | 
| -----------------------| 
| **stack**              | 
| **static (global)**    | 
| **code (text)**        | 


Assuming the following code:

```c++
// declaration of global variables
int G

int main()
{
    // declaration of local variables
    int A = funA();
    return A;
}

int function funA()
{
    // declaration of local variables
    int localA = 0;
    return localA;
}
```

When C++ enters a function or block of code, it creates a stack containing its local variables. When C++ exits a function or block of code, the corresponding stack is removed. 

So the memory looks like the followings when:

1. in `main` before entering `funA`:  

| **heap**         |      | 
| -----------------|------| 
| **stack main**   | A    | 
| **static**       | G    | 
| **code**         |      | 
 
2. in `main` after entering `funA`:  

| **heap**         |      | 
| -----------------|------| 
| **stack funA**   |localA| 
| **stack main**   | A    | 
| **static:**      | G    |
| **code**         |      | 


3. in `main` returning from `funA`:  

| **heap**         |      | 
| -----------------|------| 
| **stack main**   | A    | 
| **static**       | G    |
| **code**         |      | 

* When C++ is in `funA`, `main`'s variables exists, but they are out of **scope**. We don't have access to them.
* When `funA` is called: C++ make a stack for `funA`, does stuff, return (pass) the answer to the parent function. Passing is by either copying or address.  
    * *Pass data by a variable*: C++ copy returning value to parent function stack.
    * *Pass data by a pointer*: C++ pass the address of returning value to parent function stack.

## Heap
C++ use *dynamic memory allocation* to allocate memory during program execution (run time).

`new` command is used for dynamic allocation. It 
1. allocates memory dynamically on the heap 
2. return the address of the allocated memory

So we need a pointer to hold the address.
The pointer itself exists on the stack, while holds address of memory location on the heap. 
```c++
int* p = new int;
```
* `<type>*` declare pointer to a `<type>` value.

| **heap**         | **[?]**  | 
| -----------------|----------| 
| **stack main**   | **p**    | 

So far the memory location on the heap is just an address, it does not have value. To assign a value to the address on the heap:
```C++
*p = 50;
```
* `*<pointer>` gives access to the value of the pointer.

| **heap**         | **[50]** | 
| -----------------|----------| 
| **stack main**   | **p**    | 


Allocated memory on heap is not deallocated automatically. A common mistake is that when a function ends the pointer automatically deletes. But the memory location on heap that it used to point is still there. Dynamically allocated memory should be freed before the pointer gets deleted.

To free the dynamically allocated memory pointed by a pointer:
```C++
delete p
```
* `delete` free the memory that `p` is pointing to. 

`p` itself still exists and points to nothing (dangling pointer). It is a common practice to assign `NULL` to `p` until a new location is assigned to it.

We can assign new value to it:
```C++
*p = 100;
```
To dynamically allocate **block of memory**: 
```C++
int* q = new int[10];
```
It dynamically allocates memory for 10 integers and returns pointer to the first element of the sequence, which is assigned to `p`. 

And access it by (similar to statically allocated array, no asterisk):
```C++
q[i] = i; 
```
To free the dynamically allocated array pointed by a pointer:
```C++
delete[] q; 
```


# Declaration, Definition
Functions should be defined before being used, similar to variables.

```C++
// function declaration
int funA(int);

int main()
{
    int I = 1;
    int A = funA(int)
    return A;
}
// function definition
int funA(int localA)
{
    return 2*localA;
}
```


# Files structure
* `main.cpp` file for run the program. To include declaration use `#include xxx.hpp`
* `xxx.hpp` file/files for function declaration, initialize constants, ... . To avoid multiple declarations header files has the following structure:
```C++
#ifndef MYFUNCTION
#define MYFUNCTION
// declarations
#endif
```
* `xxx.cpp` file/files definition of functions.

# Object Oriented 
**Class** is blueprint/template. It defines something in terms of *data (instance variables)* and *operations (instance methods)*.

**Object** is instance of a class.

**Constructors** is a special method that *instantiate (create)* and *initialize* object.

# Pointer
**Pointer** is a data type that *points* to another values stored in memory. It accept *address*.

|         | Type | Name | value | address |
| ------- | -----|------|-------| ------- |
| integer | int  | x    | 50    | xxxx    |
| pointer | int* | p    | xxxx  | yyyy    |

`&` is *address of* `<a non-pointer>`. 

`*` is *content of* `<a pointer>`. Also called *dereferencing*.

```C++
int x = 25;
int* p = &x;

// x and *p are identical
x = x + 5;
x = *p + 5;
*p = *p + 5;
```
# Casting

By default `<int>/<int>` is `<int>` and `<double>/<double>` is `<double>`. But casting can be used to change these:
```C++
    <double> = (double) <int>/<int>
    <int> = (int) <double>/<double>

```

