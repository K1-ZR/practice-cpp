# Memory allocation, stack, heap

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


Assuming the following code"

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

So the memory looks the followings when:

1. in `main` before entering `funA`:  

| **heap**               | 
| -----------------------| 
| **stack main: A**      | 
| **static: G**          |
| **code**               | 

2. in `main` after entering `funA`:  

| **heap**               | 
| -----------------------| 
| **stack funA: localA** | 
| **stack main: A**      | 
| **static: G**          | 
| **code**               | 

3. in `main` returning from `funA`:  

| **heap**               | 
| -----------------------| 
| **stack main: A**      | 
| **static: G**          | 
| **code**               | 

* When C++ is in `funA`, `main`'s variables exists, but they are out of **scope**. We don't have access to them.
* When `funA` is called: C++ make a stack for `funA`, does stuff, return (pass) the answer to the parent function. Passing is by either copying or address. 

## Heap
