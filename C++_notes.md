# C++ Notes

## Notes from A Tour of C++ (2013, Stroustrup)

* C++ is a statically typed language = type of every entity must be known to compiler at its point of use
* Entity: Core Language feature (`if`, `while`, `char`), or Standard Library Component (`vector`, `map`, `getline()`)
* a function cannot be called unless it has been previously defined
* when function overloading, each function should implement the same semantics

* declaration - statement, introduces name into program
* type - defines set of possible values and operations for an object
* object - some memory that holds a value of some type
* value - set of bits interpreted according to a type
* variable - named object

* fundamental types - correspond directly to hardware facilities, fixed size
* `char` - typically 8-bit byte, other types measures in number of `char`s

#### boolean ops:
* `&` - bitwise and
* `|` - bitwise or
* `^` - bitwise exclusive or (xor)
* `~` - bitwise complement
* `&&` - logical and
* `||` - logical or

#### Initialization:
* can use equals sign or curly brace
```C++
vector<int> v {1, 2, 3, 4, 5};
complex<double> d = 1;`
complex<double> d2 = {1, 2} // the = is optional
```

* implicit narrowing conversions (losing precision) is allowed to maintain C compatibility
* using the {} notation will save you from accidentally losing precision, throwing errors

* don’t actually need to specify type when defining variable when it can be deduced from initializer:
```C++
auto b = true;
auto i = 123;
auto z = sqrt(y); // z has the type of whatever sqrt(y) returns;
```

* with auto, use the = because no potentially troublesome type conversion involved;
    - use auto when there is no specific reason to mention type explicitly
    - definition may be in large scope, so make type clearly visible
    - want to be explicit about variables range or precision (`double` vs `float`)
    - use auto to reduce redundancy, avoid long type names

#### A note on increment/decrement:
* in a for loop, `i++` and `++i` could be used interchangeably as an incrementation step
* however, `i++` is a larger instruction, as a temporary variable has to be created
* it is better to get in the habit of writing `++i` then, to avoid larger instructions, and match other higher quality code

#### Scope and Lifetime:

* local scope - name declared in a function/lambda = local name
    - extends from declaration to the end of the {} block it is declared in
    - a function's argument names are considered local names
* class scope - member names are defined in a class outside of functions/lambdas and enum classes
    - extends from declaration to }
* namespace scope - namespace member names are defined outside of classes, function/lambdas, or enum classes
    - extends from point of declaration to end of namespace
* global name/scope - any name not declared in any other construct

* an object must be initialized before it is used and will be destroyed at the end of its scope
* for a namespace object that scope is the end of the program
* for a member, the destruction occurs when the object it is a member of is destroyed
* an object created by `new` will live on until destroyed by `delete`

#### Constants:
* `const` - I will not change this value
    - specific interfaces, no fear of passing in objects that will get changed
    - compiler enforced
* `constexpr` - to be evaluated at compile time
* specify constants, to allow placement of data in read only memory, and for performance
* example:
```C++
double sum(const vector<double>&);   // sum will not modify argument
vector<double> v {1.2, 3.4, 5.6};	// v is not a constant
const double s1 = sum(v);			// ok, evaluated at runtime
constexpr double s2 = sum(v);		// error, sum is not a constant
```

* to be a `constexpr`, a function should be simple, just a `return` statement computing a value
* `constexpr` can be used for non-constant arguments, but result is not constant
* `constexpr` function allowed to be called with non-constant-expression arguments in contexts that do not require constant expressions
* this so we don’t have to define `const` expression and variable expression

* notion of immutability is important design concern

#### Pointers, Arrays References:
```C++
char v[6]; 	// array of 6 char elements, v[0] to v[5]
char* p;	// pointer to character
```
* size of array must be a constant expression
* pointer variable can hold address of an appropriately typed object

```C++
char* p = &v[3];
char x = *p;
```
* `*p` is contents of p
* `&p` is address of p


* range-for statement: `for (auto x : v){ // do something }`
* can be used for any sequence of elements
* to simply refer to the elements, could do: `for (auto& x : v){ ++x; }`
* in a declaration, `&` is called a reference, no `*` needed to access value
* a reference is simply a name that refers to an existing object
* `void sort(vector<double>& v);` ensures the vector passed in is actually sorted, value is not copied
* to avoid modifying argument, AND avoid copy cost, use `const`:
* `double sum(const vector<double>&)`
* declarator operators: operators used in declarations (`&`, `*`, `[]`)


* `nullptr` = no object is available
* often wise to check that a pointer actually points to something (ie. is not a `nullptr`)


* some tests:
* `while (*p)` = testing for `while (*p != 0)`
* `while (p)` = testing for `while (p != nullptr)`


#### Advice:
* keep functions short
* functions should performa single, logical operation
* avoid all caps names
* declare one name only per declaration
* use overloading when functions perform conceptually the same task on different types
* if a function may have to be evaluated at compile time, use `constexpr`
* state intent in comments
* avoid narrowing conversions

## User Defined Types

* builtins: types built from fundemental types, `const`, declarator operators
* low level, reflect capabilities of hardware
* C++ provides abstraction mechanisms to build high level facilities
* classes, enumerations

#### Structures

* first step, organizing elements new type needs into a data structure, `struct`

```C++
struct Vector {
    int sz;       // number of elements
    double* elem; // pointer to elements
};
```


* define like `Vector v;`
* use `.` notation to access `struct` members through a name
* however, don't reinvent standard library components, use them

#### Classes
* provides a tighter connection between the representation and operations of a type
* classes have members, can be data, functions, or type members

```C++
class Vector {
public:
    Vector(int s) : elem{new double[s]}, sz{s} {} // construct vector
    double& operator[](int i) { return elem[i]; }
    int size() { return sz; }
private:
    double* elem;
    int sz;
};
```

* define like `Vector v(6);`
* essentially, Vector is a fixed-size 'handle' for holding a variable sized amount of data
* data being allocated on the free store by `new`
* the constructor for the class is a function with the same name as the `class`
* at this point, missing error handling and destructors


* a `struct` is simply a `class` with public members be default
* can define constructors and member functions for a `struct`

#### Unions

* a `union` is a `struct` in which all members are allocated at the same address, so the union only has to hold as much space as its largest member
* can hold only one member at a time
* example:


```C++
struct Entry {
    char* name;
    char* s;    // use s for one purpose
    int i;      // use i for an exclusive other purpose
};
```

* recover wasted space by declaring a `union` of the values

```C++
union Value {
    char* s;
    int i;
};
```

* now Entry looks like this:

```C++
struct Entry {
    char* name;
    Value v;
};

void f (Entry* p){
    cout << p->v.s;
}
```

* language does not keep track of which kind of value is held by a union, programmer must keep track
* can encapsulate unions by a type field to guarantee access to initialized variables
* called tagged unions

#### Enumerations
* used to represent small sets of integer values
* `enum class Color { red, green, blue };`
* `Color col = Color::red;`


* `enum class` has only assignment, initialization, and comparisons (`<`, `==`)
* however, an enumeration is a user-defined class, so we can define operations

```C++
Color& operator++(Color& c){
    switch(t){
    case Color::green:      return t=Color::yellow;
    case Color::yellow:     return t=Color::red;
    case Color::red:        return t=Color::green;
    }
}

Color next = ++light;
```

* to avoid explicitly qualifying enumerator names, remove `class` from `enum class`
* this will give you a plain enum, implicitly integers that start at 0

```C++
enum Color { red, green, blue };
int col = green;
```


#### Advice
* prefer `enum class`es over naked `enum`s to avoid surprises
* define operations of enumerations for safe and simple use
