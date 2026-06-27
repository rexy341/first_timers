# Chapter 2 Exercises

## [A] State True or False:

* **(a)** In C++, a structure can contain data members, as well as functions that can operate upon the data members.  
  **Answer:** True

* **(b)** In C++, a union can contain data members, as well as functions that can operate upon the data members.  
  **Answer:** True

* **(c)** If the function is defined before calling it, there is no need to mention its prototype.  
  **Answer:** True

* **(d)** It is possible to create an array references.  
  **Answer:** False

* **(e)** Once a reference is tied with a variable it cannot be tied with another variable.  
  **Answer:** True

* **(f)** A variable can be tied with several references.  
  **Answer:** True

* **(g)** In a C++ program re-definitions are not allowed whereas re-declarations are allowed.  
  **Answer:** True

* **(h)** In C++ a function call can occur even on the left-hand side of an assignment operator.  
  **Answer:** True

* **(i)** It is unsafe to return a local variable by reference.  
  **Answer:** True

* **(j)** `cin` and `cout` are objects.  
  **Answer:** True

* **(k)** C++ permits the use of anonymous structures.  
  **Answer:** True

* **(l)** A pointer of another type can be assigned to a void pointer without the need for typecasting.  
  **Answer:** True

* **(m)** The following two definitions are same:
  ```cpp
  enum grade g;
  grade g;
  ```
  **Answer:** True

* **(n)** The following two statements perform the same job:
  ```cpp
  int a = 10;
  int a(10);
  ```
  **Answer:** True

* **(o)** The following two statements perform the same job:
  ```cpp
  bool a;
  BOOL a;
  ```
  **Answer:** False

* **(p)** The following three statements perform the same job:
  ```cpp
  cout << "\n";
  cout << '\n';
  cout << endl;
  ```
  **Answer:** False

---

## [B] What will be the output of the following programs:

### (a)
```cpp
#include <iostream>
using namespace std;

int main()
{
    int i = 5;
    int &j = i;
    int p = 10;
    j = p;
    cout << endl << i << endl << j;
    p = 20;
    cout << endl << i << endl << j;
    return 0;
}
```
**Output:**
```text
10
10
10
10
```

### (b)
```cpp
#include <iostream>
using namespace std;

int main()
{
    char *p = "hello";
    char *q = p;
    cout << p << endl << q;
    q = "Good Bye";
    cout << p << endl << q;
    return 0;
}
```
**Output:**
```text
hello
hello
hello
Good Bye
```

### (c)
```cpp
#include <iostream>
using namespace std;

int i = 20;

int main()
{
    int i = 5;
    cout << i << endl << ::i;
    return 0;
}
```
**Output:**
```text
5
20
```

### (d)
```cpp
#include <iostream>
using namespace std;

int i = 20;

int main()
{
    int i = 5;
    cout << i << endl << ::i;
    {
        int i = 10;
        cout << i << endl << ::i;
    }
    return 0;
}
```
**Output:**
```text
5
20
10
20
```

### (e)
```cpp
#include <iostream>
using namespace std;

const int i = 10;

int main()
{
    const int i = 20;
    cout << i << endl << ::i;
    cout << &i << endl << &::i;
    return 0;
}
```
**Output:**
```text
20
10
[local_memory_address]
[global_memory_address]
```

### (f)
```cpp
#include <iostream>
using namespace std;

int main()
{
    int i;
    cout << sizeof(i) << endl << sizeof('i');
    return 0;
}
```
**Output:**
```text
4
1
```

### (g)
```cpp
#include <iostream>
using namespace std;

int main()
{
    for (int i = 1; i <= 10; i++)
        cout << i << endl;
    cout << i;
    return 0;
}
```
**Output:**
```text
Compilation Error
cout << i;
        ^
i is not declared in this scope.
```

---

## [C] Point out the errors, if any, in the following programs.

### (d)
**Original Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    char *p = "Hello";
    p = "Hi";
    *p = 'G';
    cout << p;
    return 0;
}
```
**Answer / Corrected Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    char *p = "Hello";
    p = "Hi";
    *p = 'G'; // error: strings are read only, attempting to change characters at index 0 of literal is invalid.
    cout << p;
    return 0;
}
```

### (e)
**Original Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    enum result { first, second, third };
    result a = first;
    int b = a;
    result c = 1;
    result d = result(1);
    return 0;
}
```
**Answer / Corrected Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    enum result { first, second, third };
    result a = first;
    int b = a;
    result c = 1; // error: invalid assignment of an integer to an enum variable, requires an explicit type cast.
    result d = result(1);
    return 0;
}
```

### (f)
**Original Code:**
```cpp
const int a = 124;
const int *sample();

int main()
{
    int *p;
    p = sample();
    return 0;
}

const int *sample()
{
    return (&a);
}
```
**Answer / Corrected Code:**
```cpp
const int a = 124;
const int *sample();

int main()
{
    int *p;
    p = sample(); // error: invalid conversion from ‘const int*’ to ‘int*’
    return 0;
}

const int *sample()
{
    return (&a);
}
```

### (g)
**Original Code:**
```cpp
#include <iostream>
using namespace std;

int a = 10;

int main()
{
    int a = 20;
    {
        int a = 30;
        cout << a << ::a << ::::a;
    }
    return 0;
}
```
**Answer / Corrected Code:**
```cpp
#include <iostream>
using namespace std;

int a = 10;

int main()
{
    int a = 20;
    {
        int a = 30;
        cout << a << ::a << ::::a; // error: expected id-expression before ‘::’ token.
    }
    return 0;
}
```

### (h)
**Original Code:**
```cpp
#include <iostream>
using namespace std;

struct emp
{
    char name;
    int age;
    float sal;
};

emp e1 = { "Amol", 21, 2345.00 };
emp e2 = { "Ajay", 19, 2300.00 };
emp &fun();

int main()
{
    fun() = e2;
    cout << endl << e1.name << endl << e1.age << endl << e1.sal;
    return 0;
}

emp &fun()
{
    emp e3 = { "Aditya", 21, 3300.75 };
    return e3;
}
```
**Answer / Corrected Code:**
```cpp
#include <iostream>
using namespace std;

struct emp
{
    char name;
    int age;
    float sal;
};

emp e1 = { "Amol", 21, 2345.00 };
emp e2 = { "Ajay", 19, 2300.00 };
emp &fun();

int main()
{
    fun() = e2; /* error: e3 is a local variable for fun(). fun() returns reference variable e3 which turns dangling since e3 is destroyed when fun exits.
                   so assigning e2 to dangling e3 gives runtime error; */
    cout << endl << e1.name << endl << e1.age << endl << e1.sal;
    return 0;
}

emp &fun()
{
    emp e3 = { "Aditya", 21, 3300.75 };
    return e3;
}
```

### (i)
**Original Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    char t[] = "String functions are simple";
    int l = strlen(t);
    cout << l;
    return 0;
}
```
**Answer / Corrected Code:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    char t[] = "String functions are simple";
    int l = strlen(t); // error: header file for string functions has not been included
    cout << l;
    return 0;
}
```

---

## [D] Answer the following:

### (a) In the following program how would you define `q`, if the first `cout` is to output "Internet" twice, whereas, the second `cout` is to output "Intranet" twice.
```cpp
#include <iostream>
using namespace std;

int main()
{
    char *p = "Internet";
    cout << p << q;
    q = "Intranet";
    cout << p << q;
    return 0;
}
```
**Answer:**
```cpp
#include <iostream>
using namespace std;

int main()
{
    char *p = "Internet";
    char *&q = p; // reference to pointer
    cout << p << q;
    q = "Intranet";
    cout << p << q;
    return 0;
}
```

### (b) If employee is a structure, REGS is a union and maritalstatus is an enum then does there exist any other way in which the following definitions can be made:
```cpp
struct employee e;
union REGS i;
enum maritalstatus m;
```
**Answer:**
```cpp
employee e;
REGS i;
maritalstatus m;
```

### (c) Can the following statements be written in any other way:
```cpp
employee *p ; 
p = ( employee * ) malloc ( sizeof ( e ) ) ; 
float q ; 
int a, b ; 
q = ( float ) a / b ;
```
**Answer:**
```cpp
employee *p = (employee *) malloc(sizeof(e));
int a, b ;
float q = ( float ) a / b ;
```

