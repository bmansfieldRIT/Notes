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
char x = \*p;
```
* `\*p` is contents of p
* `&p` is address of p
