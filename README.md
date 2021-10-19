
<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->

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

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
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

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Object Oriented Programming
* **Class** is like a blueprint. It defines behaviors/states of an object type in terms of *data (instance variables)* and *operations (instance methods)*.
* **Object** is an instance of a class.
* **Method** is a behavior of an object.

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
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
<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Loop, ... 
```c++
for (int i=0; i<10; i++)
{
  ...
}

while( a != 20 ) // tests the condition before executing the loop body
{ 
 ...
}

do
{

}while( a != 20 ) // tests the condition at the end of the loop body
```
* `break`: Terminates a loop.
* `continue`: Skips the remainder of a loop

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
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

## Casting

```C++
    double x = (double) <int_i>/<int_j>
    int    i = (int) <double_x>/<double_y>
```

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Pointer
* **Pointer** is a data type that *points* to another values stored in memory. It accept *address*. Pointer is a memory location, i.e. address of variable.

* `&<non-pointer>` is *address* of a *non-pointer*. 
* `*<pointer>` is *content* of a *pointer* is referring to. It is also called *dereferencing*.

```C++
int var = 25;
int* ptr = &var;

var = var + 5;
var = *ptr + 5;
*ptr = *ptr + 5;

int ** pptr = &ptr
**pptr = 30

printf("Value of Var = %d\n", *ptr);
printf("Value of Var = %d\n", *Var);

// pointer (address) may be different in different runs
printf("Address of Var = %p\n", ptr);
printf("Address of Var = %p\n", &var);
```

* `x` and `*p` are identical.
* because of `*`, `p` is a pointer that holds an address of a memory
* because of `int` the pointer's value(s) can be accessed as integer values

|                    | Type | Name | value | address |
| ------------------ | -----|------|-------| ------- |
| integer            | int  | x    | 50    | aaaa    |
| pointer to integer | int* | p    | aaaa  | bbbb    |


## Array
Declare array using `<type> <array_name>[<dim1>][dim2]`.

```c++
int my_vector[5]; // declaration + random initialization
int my_vector[5] = {19, 10, 8, 17, 9}; // declaration + initialization
int v[3]={};  // declaration + zero initialization
int *v; // declaration + random initialization

char my_string[6] = {'H', 'e', 'l', 'l', 'o'};
```
* The argument `my_vector` represents the address of the first element of array.
Then each element is offseted by `sizeof(<type>)`.

```c++
int my_array[2][3]; // declaration 
// In general, mat[i][j] is equivalent to *(*(mat+i)+j)
// Remember mat itself is a pointer
int  my_array[2][3] = { {2, 4, 5}, 
                        {9, 0 0} }; // declaration  + initialization
// mat[0][0] == *(*(mat+0)+0)
// mat[0][2] == *(*(mat+0)+2)
// mat[1][2] == *(*(mat+1)+2)
```

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
    // v is the address of the first element
    ptr = v; // or ptr = &v[0]
    for (int i = 0; i < 3; i++)
    {
        printf("Value of *ptr = %d\n", *ptr);
        printf("Value of ptr = %p\n\n", ptr);
        ptr++; // Increment pointer ptr by 1
    }
}
```

If `arr[0]` is stored at address `x`, then `arr[1]` is stored at `x + sizeof(int)` and so on.
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

Arrays and pointer are two different things (we can check by applying sizeof). 
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

Array's disadvantage:
- Allows a fixed number of elements to be entered which is decided at the time of declaration. ??? but how about realloc

The advantages of `vector` STL over normal arrays:
- We do not need pass size as an extra parameter when we declare a vector.
- Vectors support dynamic sizes (we do not have to initially specify size of a vector).
- We can also resize a vector.
- Vectors have many in-built function like, removing an element, etc.

## Array as Strings
Strigns can be stored in two ways: C style string or C++ style string (string calss)

### C style string
a string can be referred to using:
- a character pointer
```c++
char str[4] = "GfG"; /* Stored in stack segment
                        One extra for string terminator*/

char str[4] = {‘G’, ‘f’, ‘G’, '\0'}; /* '\0' is string terminator */

*(str+1) = 'n';      /* not an error: String is now GnG */
```

- a character array, wich has two types itself:  
  - When a string value is directly assigned to a pointer
```c++
char *str  =  "GfG"; /* “GfG” is stored in a shared read-only location,
                       but pointer str is stored in a read-write memory.
                       You can change str to point something else but cannot change value at present str.*/
*(str+1) = 'n';  /* error:  trying to modify read only memory */
```
    - When dynamic allocation is used:
```c++
char *str;
int size = 4; // one extra for ‘\0’
str = (char *)malloc(sizeof(char)*size); /* Stored in heap segment */
*(str+0) = 'G';
*(str+1) = 'f';
*(str+2) = 'G';
*(str+3) = '\0';

*(str+1) = 'n';  /* not an error: String is now GnG */
```

```c++
char *getString()
{
  char str[] = "GfG"; /* Stored in stack segment which will be removed when exit the funnction*/

  /* error: string may not be present after getString() returns */
  /* it can be solved if write static before char, i.e. static char str[] = "GfG"; */
  return str;
}
int main()
{
  printf("%s", getString());
  getchar();
  return 0;
}
```
### std::string
The length of the C++ string can be changed at runtime because of dynamic allocation of memory, similar to vectors.

## smart pointer
ince the destructor is automatically called when an object goes out of scope, the dynamically allocated memory would automatically be deleted
```c++
#include <iostream>
using namespace std;
 
template <class T>
class SmartPtr {
    T* ptr; // Actual pointer
public:
    // Constructor
    explicit SmartPtr(T* p = NULL) { ptr = p; }
    // Destructor
    ~SmartPtr() { delete (ptr); }
    // Overloading dereferncing operator
    T& operator*() { return *ptr; }
    // Overloading arrow operator
    T* operator->() { return ptr; }
};
 
int main()
{
    SmartPtr<int> ptr(new int());
    *ptr = 20;
    cout << *ptr;
    return 0;
}
```

* `std::unique_ptr` https://iamsorush.com/posts/unique-pointers-cpp/
* `std::shared_ptr` https://iamsorush.com/posts/shared-pointer-cpp/
* `std::weak_ptr` https://iamsorush.com/posts/weak-pointer-cpp/


<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# References
When a variable is declared as a reference (using `&`), it becomes an alternative name for an existing variable.
References are used to avoid copying an obj when pass to a function.
```cpp
#include<iostream>

int main()
{
  int var = 10;
  int& ref = var; // ref is a reference to vat

  ref = 20;// Value of x is now changed to 20 

  return 0;
}
```
* If a function receives a reference to a variable, the value of the variable can be modified within the function.
* Can be Combined it with `const` to avoid accidental updates of passed var.
```cpp
#include<iostream>

void swap (int& first, int& second)
{
    int temp = first;
    first = second;
    second = temp;
}

void print(const Student &a)
{
    std::cout << a << std::endl ;
}

int main()
{
    int a = 2, b = 3;
    swap( a, b );

    return 0;
}
```
## Double address operator `&&`
`int&& a` means `a` is an r-value reference. `&&` is normally only used to declare a parameter of a function and it only takes a r-value expression. 
```cpp
void foo(int&& a)
{
    // ...
}

int main()
{
    int b;
    foo(b); // Error. An rValue reference cannot be pointed to a lValue.
    foo(5); // No error
    foo(b+3); // No error

    int&& c = b; // Error An rValue reference cannot be pointed to a lValue.
    int&& d = 5; // NNo error.
}
```
<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Reference vs Pointers
## Similarities
- Both can be used to change local variables of one function inside another function.
- Both of them can be used to avoid copying of big objects when passed as arguments to functions or returned from functions.
## Differences
* A pointer can be declared as void but a reference can never be void.
```cpp
int var = 10;
void* ptr = &var; /* valid, void pointer just does not know the type that is pointing to */
void &ref = var; /* not valid */
```
* pointer has multi level, but reference has only one level
```cpp
#include <iostream>
using namespace std;

int main() {
    int i=10; 
    int *p=&i; //single pointer
    int **pp=&p; //double pointer
    // p and pp differ in the value they store or point to
    cout << " p=" << p << " pt=" << pp << "\n";
    int i=5; //simple or ordinary variable
    int &r=i;
    int &rr=r;
    // r and rr do not differ in their values, as they all refer to the same variable.
    std::cout << " r=" << r << " rr=" << rr << "\n";
 }
```
- Also, members of an object reference can be accessed with dot operator `.`, unlike pointers where arrow operator `->` is needed to access members.
- reference can not be reset or rereferenced.
- References cannot be NULL.
- A reference must be initialized when declared.

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Modifiers
* `static` modifier: It allows a local variable to maintain their values between function calls. It will exist during the life-time of the program, they just come into and go out of scope.
```c++
static int i;
```

* `const` modifier: It defines constants:
```c++
const int  I = 10;
const char NewLine = '\n';
```

`const char *ptr` : a pointer to a constant character. You cannot change the value pointed by ptr, but you can change the pointer itself. non-const pointer to a const char.
`const char *ptr` and `char const *ptr` are equivalent as position of `*` is the same. 

`char *const ptr` : a constant pointer to non-constant character. You cannot change the pointer, but can change the value pointed by pointer. 

`const char *const ptr` : a constant pointer to a constant character. You can neither change the value pointed by pointer nor the pointer itself. 
`char const *const ptr` is the same as `const char *const ptr`.

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# Functions

The parameters passed to function are called *actual parameters*. 
The parameters received by function are called *formal parameters*. 

## Pass by Value
values of actual parameters are copied to function’s formal parameters and the two types of parameters are stored in different memory locations. So any changes made inside functions are not reflected in actual parameters of caller.

## Pass by Reference
Both actual and formal parameters refer to same locations, so any changes made inside the function are actually reflected in actual parameters of caller.

```cpp
#include <stdio.h>
void foo1(int var) // gets a copy of value
{
   var = 30;
}

void foo2(int &ref) // gets its reference
{
  ref = 30;
}

void foo3(int *ptr) /* gets a pointer */
{
    *ptr = 30; /* change value of a pointer with dereferencing */
}

int main(void) {
  int b = 20;

  foo1(a);  // pass by value
  foo2(a);  // pass by reference
  foo3(&a); // pass by pointer
  return 0;
}
```

## main function
It serves as the entry point for the program, that is called by os.

```cpp
int main() // without argument
{
   ...
   return 0;
}
```
```cpp
int main(int argc, char * const argv[]) // with argument from command line
{
   ...
   return 0;
}
```
<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# C style

`memcpy( &dst[dstIdx], &src[srcIdx], numElementsToCopy * sizeof( Element ) );`

```cpp
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
```
```cpp
  int *ptr = (int *)malloc(sizeof(int)*2);
  int i;
  int *ptr_new;

  *ptr = 10;
  *(ptr + 1) = 20;

  ptr_new = (int *)realloc(ptr, sizeof(int)*3);// usually to increase size
  *(ptr_new + 2) = 30;
  for(i = 0; i < 3; i++)
    printf("%d ", *(ptr_new + i));
```

<!-- ####################################################################### -->
<!-- ####################################################################### -->
<!-- ####################################################################### -->
# STL
## std::string
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

    str6.append(" extension");// append add the argument string at the end
                              //  same as str6 += " extension"

    //  find returns index where pattern is found.
    //  If pattern is not there it returns constant npos whose value is -1
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

# std::pair
The first element is referenced as ‘first’ and the second element as ‘second’ and the order is fixed (first, second).
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

# Uncategorized
https://en.wikipedia.org/wiki/Data_segment