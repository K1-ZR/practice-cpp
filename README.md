
[Memory Allocation](#memory-allocation) --
[Declaration, Definition](#declaration-definition) --
[Files structure](#files-structure) --
[Object Oriented Programming](#object-oriented-programming) --
[Pointer](#pointer) --
[Functions](#functions) --
[Class](#class) --
[Variable](#variable) --
[Casting](#casting) --
[Operator](#operator) --
[Array](#array) --
[multidimensional array](#multidimensional-array) --
[Passing to a function](#passing-to-a-function) --
[Loop](#loop) --
[String](#string) --
[Number](#number) --
[IO](#io) --
[Run a C++](#run-a-c) --
<!--  -->
<!--  -->
<!--  -->
# Memory Allocation

* **Scope**: where in a program a variable can be referenced.
* **Lifetime**: how long a variable exist in memory.
* **Local variables**: variables that are declared within a function or block of code. 
*Block of code* can be a loop, a condition, etc. (basically between `{}`).
Local variable's  *scope* is limited from the point of declaration to the end of the function or the block of code in which they are declared.  
Their *lifetime* is from entering a function or a block of code in which they are declared to termination of it.  
* **Global variables:** variables declared outside of any function.  
Global variable's *scope* is anywhere in the program.  
Their *lifetime* is the duration of the program.  


## Stack
`C++` uses the following memory allocation: 
* Heap (free store)
* Stack 
* Static (for global variables)

To understand how it works, let's assume the following code:

```c++
#include <stdio.h>

int G; // global variables
int funA(); // function decleration before being used, similar to variables.


int main()
{
    int A = funA(); //local variables
    return 0;
}

int funA()
{
    int B = 10; // local variables
    return B;
}
```
First `C++` creates the *static* to store global variables.
Also, when `C++` enters a function or a block of code, it creates a *stack* containing its local variables. When `C++` exits a function or a block of code, it removes the corresponding stack. 

<img src="images/stack.png" width="100%">

**Notes**:
* When `C++` enters a function, parent function's variables exists, but they are out of *scope*. So the user does not have access to them.
* When a function is called, `C++` makes a stack for that function, does the calculation, return the answer to the parent function. Passing is by either *copying* or *address*.  
    * *Pass data by a variable*: C++ copy returning value to parent function stack.
    * *Pass data by a pointer*: C++ pass the address of returning value to parent function stack.

## Heap
C++ use *dynamic memory allocation* to allocate memory during program run time.
The `new` command is used for dynamic allocation. It 
1. Allocates memory dynamically on the heap 
2. Returns the *address* of the allocated memory
3. Does **not** initialize the location with a value

So a *pointer* is needed to hold the *address*.
The *pointer* itself exists on the *stack*, while holds address of a memory location on the heap. 

<img src="images/heap.png" width="100%">

* `<type>*` declare pointer to the `<type>` value.
* `*<pointer>` gives access to the value of the pointer.
* Allocated memory on heap is not deallocated automatically. A common mistake is that when a function ends the pointer automatically vanishes, but the memory location on heap that it used to point is still there. Dynamically allocated memory should be freed using `delete` before the pointer gets deleted.
* `p` itself still exists and points to nothing (dangling pointer). A common practice is to assign `NULL` to `p` after `delete`.
```C++
int* p = new int;
*p = 50;
delete p;
p = NULL;
```
To dynamically allocate a *block of memory*: 
```C++
int* q = new int[10];
q[i] = 0; 
delete[] q; 

```
* It dynamically allocates memory for 10 integers and returns the pointer to the first element of the sequence. 
* Use `[]` to access its memebers.
* To free the dynamically allocated array (in the heap) pointed by a pointer use `delete[]`.

NOTE: never delete an array that is in the run-time stack. 
```
int a[100];
...
delete [] a; // this corrupt the heap manager
```

<!--  -->
<!--  -->
<!--  -->

# Files structure
* Create a `main.cpp` file for running the program.
* Use *header* files, `<class_name>.hpp`, for class declaration, initialize constants, etc.
* Use `#include <function>.hpp` to include declarations in `main.cpp`.
* Use `<class_name>.cpp` files for class definition.

To avoid multiple declarations, *header* file has the following structure:
```C++
#ifndef MYFOO
#define MYFOO
// declarations
#endif
```

<!-- ################################################################### -->
<!--  -->
<!--  -->

# Object Oriented Programming
* **Class** is like a blueprint. It defines behaviors/states of an object type in terms of *data (instance variables)* and *operations (instance methods)*.
* **Object** is an instance of a class.
* **Method** is a behavior of an object.

<!--  -->
<!--  -->
<!--  -->

# Pointer
* **Pointer** is a data type that *points* to another values stored in memory. It accept *address*.
* `&<non-pointer>` is *address* of a *non-pointer*. 
* `*<pointer>` is *content* of a *pointer* is referring to. It is also called *dereferencing*.

```C++
int X = 25;
int* P = &X;

x = x + 5;
x = *p + 5;
*p = *p + 5;
```
* `x` and `*p` are identical.

|                    | Type | Name | value | address |
| ------------------ | -----|------|-------| ------- |
| integer            | int  | x    | 50    | aaaa    |
| pointer to integer | int* | p    | aaaa  | bbbb    |

<!--  -->
<!--  -->
<!--  -->

# Class
```c++
class <class_name> {
  <access_specifier>:
    <type> <member_name>; // declerations
    ...
};
```

The *access specifiers*: 
* `private`: accessible only from within other members of the same class (*friends*).
* `protected`: accessible from other members of the same class (*friends*), also from members of their derived classes.
* `public`: members are accessible from anywhere where the object is visible.


```c++
#include <iostream>

class Rectangle {
  private:
    int width, height;
  public:
    void set_values (int,int);
    int area() {return width*height;}
};

void Rectangle::set_values(int x, int y){
  width = x;
  height = y;
}

int main(){
  Rectangle rect;
  rect.set_values (3,4);
  std::cout << "area: " << rect.area();
  return 0;
}
```

# Variable
| Type           | Keyword                       | Size
| ---            | ---                           | ---
|Boolean         | `bool`                        | 1 byte
|Character       | `char`                        | 1 byte
|Integer         | `[signed/unsigned] int`       | 4 bytes
|                | `[signed/unsigned] short int` | 2 bytes
|Floating        | `float`                       | 4 bytes
|Double floating | `double`                      | 8 bytes
|Valueless       | `void`                        |


* `static` modifier:
It allows the local variables to maintain their values between function calls.
They will exist during the life-time of the program, they just come into and go out of scope.
The static modifier may also be applied to global variables.
When this is done, it causes that variable's scope to be restricted to the file in which it is declared.

```c++
static int i;
```

* `extern` modifier:
It gives a reference of a global variable that is visible to ALL the program files.
It is used when there are two or more files sharing the same global variables or functions.

```c++
// First File: main.cpp
#include <iostream>

int count ;
extern void write_extern();

main() {
   count = 5;
   write_extern();
}

// Second File: support.cpp
#include <iostream>

extern int count;

void write_extern(void) {
   std::cout << "Count is " << count << std::endl;
}
```

Compile these two files as follows
```shell
$ g++ main.cpp support.cpp -o write
```

* `const` modifier: it defines constants:
```c++
const int  I = 10;
const char NewLine = '\n';
```

<!--  -->
<!--  -->
<!--  -->
# Casting

By default `<int>/<int>` is `<int>` and `<double>/<double>` is `<double>`. But casting can be used to change these:
```C++
    <double> = (double) <int>/<int>
    <int> = (int) <double>/<double>
```
# Operator	
`C ?= A` is equivalent to `C = C ? A`.

# Array

* A vector should not be passed by *value* (the whole vector will be copied and passed as argument), should be passed by **reference** (`vector<char>& my_array`) instead.

Declare array using `<dataType> <arrayName>[<arraySize>]`.

```c++
float my_vector[5];
```
* The argument `my_vector` represents the memory address of first element of array `my_vector[5]`.

Accessing array members by `<arrayName>[<i>]`.

* Suppose the starting address of mark[0] is 2120d. Then, the next address, a[1], will be 2124d, address of a[2] will be 2128d and so on. It's because the size of a float is 4 bytes.

Initializing array:
```c++
int my_vector[5] = {19, 10, 8, 17, 9};
```

## multidimensional array
Declare:
```c++
int x[2][3];
```
Initialize:
```c++
int  test[2][3] = { {2, 4, 5}, {9, 0 0}};
```

## Passing to a function
```c++
void fun(int m[5]){
    ...
}

fun(marks)
```
?????????
And the formal argument int `m[5]` in function declaration converts to `int* m`;
This pointer points to the same address pointed by the array marks.

```c++
#include <vector>
std::vector<int> myvector(size);
std::vector<int> myvector;

myvector.push_back(n);
myvector.size();
myvector[i] = n;
myvector = {1,2,3,4,5};

myvector.begin();
myvector.end();

vec.erase(vec.begin() + 1); // remove member 1
vec.clear(); // remove all members

// matrices
vector< vector<int> > matrix = { { 0, 1 }, 
                                 { 1, 0 } }; 
matrix[i][j]
```


<!--  -->
<!--  -->
<!--  -->
# Loop 
* `for`:
```c++
for (int i=0; i<10; i++){
  ...
}
```
* `while`: 
  Repeats a statement/statements while a given condition is true. It tests the condition before executing the loop body.
```c++
while( a != 20 ){
 ...
}
```
* `do...while`: 
  Like a *while* statement, except that it tests the condition at the end of the loop body.
* `break`:
  Terminates a loop.
* `continue`:
  Skips the remainder of a loop
  and immediately retest its condition prior to reiterating.

# String
```c++
#include <string>

char greeting[6] = {'H', 'e', 'l', 'l', 'o'};

std::string str1 = greeting;
std::string str2 = "Hello";
str3 = str1 + str2;
str1[3]=str2[1];

// vector to string
std::vector<char> v;
std::string str(v.begin(),v.end());

// pair
std::pair <int, char> PAIR1 ; 
PAIR1.first = 100; 
PAIR1.second = 'G'; 
```

# Number
```c++
int division = (int) a / b;
// 0 = (int)2/3;
int remainder =a % b;
```
# IO
```c++
std::cout << "hello" << a << std::endl;
```
<!--  -->
<!--  -->
<!--  -->
# Run a C++
```shell
$ g++ hello.cpp
$ ./a.out
```

# Pointer
Pointer is memory location, i.e. address of variable.
```cpp
{datatype} *{var_name};
int      *ptr;
```
1. because of `*`, `ptr` is a pointer that holds an address of a memory
2. because of `int` the pointer's value(s) can be accessed as integer values

- the operator `&` returns the address of a variable.
- the p[erator `*` is used for:
    - To declare a pointer variable: `int *ptr;`
    - To access the value stored in an memory, also called Deferencing: `*ptr = 20;`

```cpp
#include <iostream>
int main()
{
  int var = 10;
  int *ptr = &var;
  *ptr = 20;

  std::cout << "value of var=" << var << std::endl;
  std::cout << "address of var=" << ptr << std::endl;

  int ** ptrr = &ptr
  **ptrr = 30
  std::cout << "value of var=" << var << std::endl;

  return 0;
}
```

```cpp
#include <stdio.h>
int main()
{
    int Var = 10;
     int *ptr = &Var;

    printf("Value of Var = %d\n", *ptr);
    printf("Value of Var = %d\n", *Var);

    // The output of this line may be different in different runs
    printf("Address of Var = %p\n", ptr);
    printf("Address of Var = %p\n", &var);

    *ptr = 20; // Value at address is now 20
    printf("Address of Var = %d\n", *ptr);

    return 0;
}
```
## Array and pointer
When Pointers are representing an array, some arithmetic operations can be done on pointers:
- incremented `++`
- decremented `-`
- an integer may be added to a pointer `+` or `+=`
- an integer may be subtracted from a pointer `–` or `-=`

```cpp
#include <bits/stdc++.h>
int main()
{
    int v[3] = {10, 100, 200};

    int *ptr;

    // Assign the address of v[0] to ptr
    ptr = v; // or ptr = &v[0]

    for (int i = 0; i < 3; i++)
    {
        printf("Value of *ptr = %d\n", *ptr);
        printf("Value of ptr = %p\n\n", ptr);
        // Increment pointer ptr by 1
        ptr++;
    }
    // remember ptr = v so:
    // ptr[1] == v[1] ...
}
```

An array name acts as a pointer which is equal to the address of the first element. i.g. `v == &v[0]`
```cpp
// In general, mat[i][j] is equivalent to *(*(mat+i)+j)
// Remember mat itself is a pointer
int mat[2][3]  =  { {16, 18, 20}, {25, 26, 27} };
// mat[0][0] == *(*(mat+0)+0)
// mat[0][2] == *(*(mat+0)+2)
// mat[1][2] == *(*(mat+1)+2)
```

# Array
Array decleration:
- `int v[3];`, random initialization
- `int v[3]={1,2,3};`
- `int v[3]={};`, zero initialization
- `int *v;`, random initialization

Notes:
The elements are stored at contiguous memory locations.
```cpp
#include <stdio.h>
int main()
{
    int arr[5], i;

    printf("Size of integer in this compiler is %lu\n",
           sizeof(int));

    for (i = 0; i < 5; i++)
        printf("Address arr[%d] is %p\n", i, &arr[i]);

    return 0;
}
```
If `arr[0]` is stored at address `x`, then `arr[1]` is stored at `x + sizeof(int)` and so on.

Array's disadvantage:
- Allows a fixed number of elements to be entered which is decided at the time of declaration. Unlike a linked list, an array in C is not dynamic.
- Insertion and deletion of elements can be costly since the elements are needed to be managed in accordance with the new memory allocation.

Arrays and pointer are two different things (we can check by applying sizeof). Array name indicates the address of first element and arrays are always passed as pointers (even if we use square bracket).
```cpp
#include <iostream>
using namespace std;

int main()
{
    int arr[5];
    int* ptr;

    // sizof(int) * (number of element in arr[]) is printed
    cout << "Size of arr[] " << sizeof(arr) << "\n";

    // sizeof a pointer is printed which is same for all
    // type of pointers (char *, void *, etc)
    cout << "Size of ptr " << sizeof(ptr);
    return 0;
}
```

The advantages of `vector` STL over normal arrays:
- We do not need pass size as an extra parameter when we declare a vector i.e, Vectors support dynamic sizes (we do not have to initially specify size of a vector).
- We can also resize a vector.
- Vectors have many in-built function like, removing an element, etc.

# References
When a variable is declared as a reference, it becomes an alternative name for an existing variable.
A variable can be declared as a reference by putting `&` in the declaration.
```cpp
#include<iostream>

int main()
{
  int x = 10;

  int& ref = x; /* ref is a reference to x. */

  ref = 20;/* Value of x is now changed to 20 */
  std::cout << "x = " << x << std::endl ;

  return 0;
}
```
- Modify the passed parameters in a function: If a function receives a reference to a variable, it can modify the value of the variable.
```cpp

#include<iostream>

void swap (int& first, int& second)
{
    int temp = first;
    first = second;
    second = temp;
}

int main()
{
    int a = 2, b = 3;
    swap( a, b );
    stdstd::cout << a << " " << b;
    return 0;
}
```
- Avoiding a copy of large structures: If we pass an object without reference, a new copy of it is created which wastes CPU time and memory.
 We can use references to avoid this. Can be Combined it with `const` to avoid accidental updates of passed var.
 ```cpp
 struct Student {
   string name;
   string address;
   int rollNo;
}

void print(const Student &s)
{
    cout << s.name << "  " << s.address << "  " << s.rollNo;
}
 ```
# Reference vs Pointers
## Similarities
- Both can be used to change local variables of one function inside another function.
- Both of them can be used to avoid copying of big objects when passed as arguments to functions or returned from functions.
## Differences
- A pointer can be declared as void but a reference can never be void For example
```cpp
int a = 10;
void* aa = &a;. /* valid */
void &ar = a; /* not valid */
```
- pointer has multi level, but reference has only one level
```cpp
#include <iostream>
using namespace std;

int main() {
    int i=10; //simple or ordinary variable.
    int *p=&i; //single pointer
    int **pp=&p; //double pointer
    // All the above pointers differ in the value they store or point to.
    cout << "i=" << i << " p=" << p << " pt=" << pp << "\n";
    int i=5; //simple or ordinary variable
    int &r=i;
    int &rr=r;
    // All the above references do not differ in their values
    // as they all refer to the same variable.
    std::cout << "i=" << i << " r=" << r " rr=" << rr << "\n";
 }
```
- Also, members of an object reference can be accessed with dot operator `.`, unlike pointers where arrow operator `->` is needed to access members.
- reference can not be reset or rereferenced
- References cannot be NULL.
- A reference must be initialized when declared.

# Strings
Strigns can be stored in two ways: C style string or C++ style string (string calss)

## C style string
a string can be referred to using:
- 1 a character pointer
They are stored like other types of arrays in C. For example, if str[] is an auto variable then string is stored in stack segment, if it’s a global or static variable then stored in data segment, etc.

```c
char str[4] = "GfG"; /*One extra for string terminator*/
/*    OR    */
char str[4] = {‘G’, ‘f’, ‘G’, '\0'}; /* '\0' is string terminator */
```
- 2 a character array, wich has two types itself:
    - 2.1 When a string value is directly assigned to a pointer, it’s stored in a read-only block that is shared among functions.
```c++
char *str  =  "GfG"; /* “GfG” is stored in a shared read-only location,
                       but pointer str is stored in a read-write memory.
                       You can change str to point something else but cannot change value at present str.*/
```
    - 2.2 When dynamic allocation is used:
```c++
char *str;
int size = 4; /*one extra for ‘\0’*/
str = (char *)malloc(sizeof(char)*size);
*(str+0) = 'G';
*(str+1) = 'f';
*(str+2) = 'G';
*(str+3) = '\0';
```
Examples:
```c++
int main()
{
 char *str;
 str = "GfG";     /* Stored in read only part of data segment */
 *(str+1) = 'n';  /* Problem:  trying to modify read only memory */
 return 0;
}
```
```c++
int main()
{
 char str[] = "GfG";  /* Stored in stack segment like other auto variables */
 *(str+1) = 'n';      /* No problem: String is now GnG */
 getchar();
 return 0;
}
```
```c++
int main()
{
  int size = 4;
  /* Stored in heap segment like other dynamically allocated things */
  char *str = (char *)malloc(sizeof(char)*size);
  *(str+0) = 'G';
  *(str+1) = 'f';
  *(str+2) = 'G';
  *(str+3) = '\0';
  *(str+1) = 'n';  /* No problem: String is now GnG */
   getchar();
   return 0;
}
```
```c++
char *getString()
{
  char *str = "GfG"; /* Stored in read only part of shared segment */

  /* No problem: remains at address str after getString() returns*/
  return str;
}

int main()
{
  printf("%s", getString());
  getchar();
  return 0;
}
```
```c++
char *getString()
{
  int size = 4;
  char *str = (char *)malloc(sizeof(char)*size); /*Stored in heap segment*/
  *(str+0) = 'G';
  *(str+1) = 'f';
  *(str+2) = 'G';
  *(str+3) = '\0';

  /* No problem: string remains at str after getString() returns */
  return str;
}
int main()
{
  printf("%s", getString());
  getchar();
  return 0;
}
```

```c++
char *getString()
{
  char str[] = "GfG"; /* Stored in stack segment */

  /* Problem: string may not be present after getString() returns */
  /* Problem can be solved if write static before char, i.e. static char str[] = "GfG";*/
  return str;
}
int main()
{
  printf("%s", getString());
  getchar();
  return 0;
}
```
## C++ style (std::string calss)
The length of the C++ string can be changed at runtime because of dynamic allocation of memory similar to vectors.
```c++
#include <bits/stdc++.h>

int main()
{
    // various constructor of string class
    std::string str1("first string");// initialization by raw string
    std::string str2(str1);// initialization by another string
    std::string str3(5, '#');// initialization by character with number of occurrence

    str1.clear(); // clear function deletes all character from string

    int len = str6.length(); // Same as "len = str6.size();"

    char ch = str6.at(2); //  Same as "ch = str6[2];"

    char ch_f = str6.front();  // Same as "ch_f = str6[0];"
    char ch_b = str6.back();   // Same as "ch_b = str6[str6.length() - 1];"

    const char* charstr = str6.c_str();// c_str returns null terminated char array version of string
    printf("%s\n", charstr);

    str6.append(" extension");// append add the argument string at the end
                              //  same as str6 += " extension"

    //  find returns index where pattern is found.
    //  If pattern is not there it returns predefined
    //  constant npos whose value is -1
    if (str6.find(str4) != string::npos)
        cout << "str4 found in str6 at " << str6.find(str4)
             << " pos" << endl;
    else
        cout << "str4 not found in str6" << endl;

    //  substr(a, b) function returns a substring of b length
    //  starting from index a
    cout << str6.substr(7, 3) << endl;

    //  if second argument is not passed, string till end is
    // taken as substring
    cout << str6.substr(7) << endl;

    //  erase(a, b) deletes b characters at index a
    str6.erase(7, 4);
    cout << str6 << endl;

    //  iterator version of erase
    str6.erase(str6.begin() + 5, str6.end() - 3);
    cout << str6 << endl;

    //  replace(a, b, str)  replaces b characters from a index by str
    str6.replace(2, 7, "ese are test");

    return 0;
}
```
# Functions
```cpp
int max(int, int); /* takes two integers as parameters and returns an integer */

int *swap(int*,int);/* takes a int pointer and an int variable as parameters and returns an pointer of type int */

char *call(char b); /* takes a charas parameters and returns an reference variable */
```
The parameters passed to function are called actual parameters. For example, in the above program 10 and 20 are actual parameters.
The parameters received by function are called formal parameters. For example, in the above program x and y are formal parameters.

## Pass by Value
values of actual parameters are copied to function’s formal parameters and the two types of parameters are stored in different memory locations. So any changes made inside functions are not reflected in actual parameters of caller.
## Pass by Reference
Both actual and formal parameters refer to same locations, so any changes made inside the function are actually reflected in actual parameters of caller.
```cpp
#include <stdio.h>
void fun(int x)
{
   x = 30;
}

int main(void)
{
    int x = 20;
    fun(x);
    printf("x = %d", x); /* prints still 20 */
    return 0;
}
```

```cpp
# include <stdio.h>
void fun(int *ptr) /* gets a pointer*/
{
    *ptr = 30; /* change value of a pointer with dereferencing */
}

int main()
{
  int x = 20;
  fun(&x); /* pass address (pointer) of var x */
  printf("x = %d", x); /* prints 30 */

  return 0;
}
```
- In C, functions can return any type except arrays and functions. We can get around this limitation by returning pointer to array or pointer to function.
- good practice to declare a function that can only be called without any parameter, with`void foo(void)`.

## main function
It serves as the entry point for the program, that is called by os.

- without arguments
```cpp
int main()
{
   ...
   return 0;
}
```
- with argument
```cpp
int main(int argc, char * const argv[]) /* inputs from command line */
{
   ...
   return 0;
}
```










# Pair container
- `pair (data_type1, data_type2) Pair_name`
- The first element is referenced as ‘first’ and the second element as ‘second’ and the order is fixed (first, second).
- Pair is used to combine together two values which may be different in type. Pair provides a way to store two heterogeneous objects as a single unit.
```c++
#include <iostream>
#include <utility>

int main()
{
    std::pair<int, char> PAIR1;
    // pair<std::string, double> PAIR2("GeeksForGeeks", 1.23);
    // pair<std::string, double>g2 = {'a', 1};
    // pair<int, pair<int, char> > pair3 = { 3, { 4, 'a' } };

    PAIR1.first = 100;
    PAIR1.second = 'G';

    cout << PAIR1.first << " ";
    cout << PAIR1.second << endl;

    return 0;
}
```
```cpp
/* correct */
myPointer = new int;
delete myPointer; //freed memory
myPointer = NULL; //pointed dangling ptr to NULL
```
```cpp
/* wrong */
myPointer = new int;
myPointer = NULL; //leaked memory, no pointer to above int
delete myPointer; //no point at all
```

pointer + size
gives pointer to next var in an array or struct


# Uncategorized
https://en.wikipedia.org/wiki/Data_segment

memcpy( &dst[dstIdx], &src[srcIdx], numElementsToCopy * sizeof( Element ) );


int main()
{
    int arr[10] = {8,3,11,61,-22,7,-6,2,13,47};
    int new_arr[5];

    memcpy(new_arr,arr,sizeof(int)*5);
    cout << "After copying" << endl;
    for (int i=0; i<5; i++)
        cout << new_arr[i] << endl;
    return 0;
}


int *ptr = (int *)malloc(sizeof(int)*2);
   int i;
   int *ptr_new;

   *ptr = 10;
   *(ptr + 1) = 20;

   ptr_new = (int *)realloc(ptr, sizeof(int)*3);
   *(ptr_new + 2) = 30;
   for(i = 0; i < 3; i++)
       printf("%d ", *(ptr_new + i));
