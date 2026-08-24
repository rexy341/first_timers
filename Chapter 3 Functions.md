# C++ Functions & Overloading — Practice Q&A

Topics covered: function prototypes, function overloading, default arguments, inline functions, and operator overloading.

---

## A. True or False

| # | Statement | Answer |
|---|-----------|:------:|
| a | If a function is defined before calling it, there is no need to mention its prototype. | **False** |
| b | Two functions can be overloaded if their arguments are similar but their return values are different. | **False** |
| c | Two functions can be overloaded only if their arguments differ in number, order, type. | **True** |
| d | If default values are mentioned for the four arguments in the function's prototype, we can call this function and pass it the first and fourth argument. | **False** |
| e | A function can be overloaded any number of times. | **True** |
| f | The assignment operator cannot be overloaded. | **False** |
| g | When we define the function to be inline there is no guarantee that its code would get inserted at the place where the call is being made. | **True** |
| h | The side effects of the macro definition get eliminated if we use inline functions. | **True** |

---

## B. Spot the Errors

### (a) Missing forward declaration

```cpp
void f(); // forward declaration of function is required if prototype
          // is not defined in scope before function call

void main()
{
    int a = 30;
    f();
}

void f()
{
    int b = 20;
}
```

### (b) Deprecated header / missing `using namespace` / wrong `main` return type

```cpp
#include <iostream>       // <iostream.h> is deprecated in modern C++ compilers
using namespace std;       // cout lives inside the std namespace; using it
                            // globally without this causes an
                            // "undeclared identifier" compiler error

void f()
{
    cout << "Hello";
}

int main()                 // C++ standard mandates main must return int
{
    f();
}
```

### (c) Return type mismatch between declaration and definition

```cpp
#include <iostream>
using namespace std;

int f(int, int);
int f(int, int);

int main()
{
    int a;
    a = f(10, 30);
    cout << a;
}

void f(int x, int y)   // new declaration of f() as void is ambiguating;
                        // also, since the function returns an int value,
                        // the return type must be int
{
    return x + y;
}
```

### (d) Local prototype scope issue

```cpp
#include <iostream>
using namespace std;

void fun1(void);
void fun2(void);

int main()              // C++ standard mandates main must return int
{
    fun1();
}

void fun1(void)
{
    fun2();              // if the forward declaration is inside main()'s
                          // scope, it will not be visible in fun1()'s scope
    cout << endl << "Hi......Hello";
}

void fun2(void)
{
    cout << endl << "to you";
}
```

### (e) Default arguments not visible at call site

```cpp
#include <iostream>
using namespace std;

void f(int, float);

void f(int i = 10, float a = 3.14)
{
    cout << i << a;
}

int main()
{
    f();   // if the function is called with no parameters after a forward
           // declaration that has no defaults, there are no default
           // parameters set — this causes a compiler error
}
```

### (f) Ambiguous overload caused by default arguments

```cpp
#include <iostream>
using namespace std;

void f(int, int, int);   // a simple forward declaration ensures the
                          // compiler distinguishes overloads by the
                          // number of parameters
void f(int, int);

int main()
{
    f(1, 2);   // ambiguous call: f(int, int, int) has default values and
               // overlaps with f(int, int)
}

void f(int x = 10, int y = 20, int z = 30)
{
    cout << endl << x << endl << y << endl << z;
}

void f(int x, int y)
{
    cout << endl << x << endl << y;
}
```

---

## C. Programs

### (a) `cls()` — clear whole screen or a given region

```cpp
#include <iostream>
#include <string>

// Global constants for terminal dimensions
const int SCREEN_WIDTH  = 80;
const int SCREEN_HEIGHT = 40;

/**
 * Clears the entire screen by printing multiple newlines.
 * Overloaded version with no arguments.
 */
void cls() {
    for (int i = 0; i < SCREEN_HEIGHT; ++i) {
        std::cout << "\n";
    }
    std::cout << std::flush;
}

/**
 * Clears a specific rectangular region of the screen using spaces.
 * @param x      starting column (0-indexed from left)
 * @param y      starting row (0-indexed from top)
 * @param width  number of columns to clear
 * @param height number of rows to clear
 */
void cls(int x, int y, int width, int height) {
    std::string spaces(width, ' ');

    for (int row = 0; row < height; ++row) {
        // Move the cursor to the target row/column using ANSI escape codes
        std::cout << (y + row + 1) << ";" << (x + 1) << "H";
        std::cout << spaces;
    }
    std::cout << std::flush;
}

int main() {
    cls();
    for (int i = 0; i < 20; ++i) {
        std::cout << "Line " << (i < 10 ? "0" : "") << i
                  << ": OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO\n";
    }

    std::cout << "\nPress Enter to clear a specific box (X:10, Y:3, W:20, H:5)...";
    std::cin.get();

    cls(10, 3, 20, 5);

    std::cout << "Press Enter to clear the entire screen...";
    std::cin.get();

    cls();
    std::cout << "Screen cleared!\n";

    return 0;
}
```

### (b) `writestring()` — display a string at row/column with default page & color

```cpp
#include <iostream>
#include <string>

void writestring(int row, int col, const std::string &str, int page = 0, int color = 7) {
    std::cout << "[--- VDU SCREEN PAGE " << page << " ---]\n";

    // Move to the correct row by printing blank lines
    for (int i = 0; i < row; i++) {
        std::cout << "\n";
    }

    // Move right to the correct column by printing spaces
    for (int j = 0; j < col; j++) {
        std::cout << " ";
    }

    // Map the requested color number (0-7) to a standard text style
    switch (color) {
        case 1:  std::cout << "\033[34m"; break; // Blue
        case 2:  std::cout << "\033[32m"; break; // Green
        case 3:  std::cout << "\033[36m"; break; // Cyan
        case 4:  std::cout << "\033[31m"; break; // Red
        case 5:  std::cout << "\033[35m"; break; // Magenta
        case 6:  std::cout << "\033[33m"; break; // Yellow
        case 7:  std::cout << "\033[37m"; break; // White (default)
        default: std::cout << "\033[0m";  break; // Reset style
    }

    std::cout << str << "\033[0m\n\n";
}

int main() {
    writestring(2, 5, "Hello", 0, 2);
    writestring(1, 12, "World");   // uses default page = 0, color = 7

    return 0;
}
```

### (c) Skip default arguments selectively

Given: `void f(int=10, int=20, int=30, int=40);`
Goal: pass the 1st and 3rd arguments while the 2nd and 4th stay at their defaults.

```cpp
#include <iostream>
using namespace std;

void f(int = 10, int = 20, int = 30, int = 40);

int main() {
    f(2, -1, 4);   // pass a sentinel for the argument to be skipped
}

void f(int a, int b, int c, int d)
{
    if (b == -1) {   // sentinel check restores the default
        b = 20;
    }

    cout << "a = " << a << ", b = " << b
         << ", c = " << c << ", d = " << d;
}
```

> Note: C++ has no built-in syntax to "skip" a positional default argument.
> The common workaround is a sentinel value (as above) or restructuring
> the function to take named/optional parameters differently (e.g. via
> overloads or a struct of options).

### (d) Overloaded `int` → ASCII and `float` → ASCII conversion

```cpp
#include <iostream>
using namespace std;

char ascii(int num)
{
    return char(num);
}

char ascii(float num)
{
    return char(int(num));
}

int main() {
    int val1;
    float val2;
    cout << endl << "Enter values to convert: ";
    cin >> val1 >> val2;

    cout << endl << "Final result is: " << ascii(val1) << " " << ascii(val2);
}
```

### (e) Overloaded ASCII string → `int` and ASCII string → `float`

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int ascii(char ch, int parameter)
{
    parameter = int(ch);
    return parameter;
}

float ascii(char ch, float parameter)
{
    parameter = float(int(ch));
    return parameter;
}

int main() {
    char val;
    cout << endl << "Enter values to convert: ";
    cin >> val;

    printf("Final result is: %d %.2f", ascii(val, 0), ascii(val, 0.0f));
}
```

### (f) Function prototypes

| Requirement | Prototype |
|---|---|
| Receives an `int` and a `float`, returns a `double` | `double f(int, float);` |
| Receives an `int` pointer and a `float` reference, returns an `int` pointer | `int *f(int *, float &);` |
| Receives nothing and returns nothing | `void f(void);` |
| Receives an array of `int`s and a `float` reference, returns nothing | `void f(int [], float &);` |

### (g) Operators that cannot be overloaded

In C++, five core operators cannot be overloaded:

- Member access operator — `.`
- Pointer-to-member operator — `.*`
- Scope resolution operator — `::`
- Ternary conditional operator — `?:`
- `sizeof` operator

### (h) Can operator overloading change operator precedence?

**No.** Operator precedence is a fixed part of the language grammar handled
by the compiler; overloading only changes what an operator *does* for a
given type, not where it sits in the precedence hierarchy.

```cpp
// case 1
a + b * (c - d)

// case 2 — with an overloaded operator+ / operator* for a `complex` type
complex c = a + b * d;
```

In both cases, `*` still binds tighter than `+`, and parentheses still
take precedence over both — regardless of overloading.

---

*Compiled as a study reference for C++ function overloading and default
arguments.*
