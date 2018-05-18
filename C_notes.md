## The C Programming Language

#### General Info:
* general purpose, imperative procedural programming language
* static type system
* supports structured programming, lexical variable scope, and recursion
* c provides constructs that efficiently map to typical machine instructions, giving rise to popularity
* designed to be compiled using a straightforward compiler
* provides low level access to memory
* requires minimal run-time support

#### History:
* Developed by Dennis Ritchie, 1969-1973 at Bell Labs
* c was invented to solve the lack of byte addressability in B (Simplified BCPL), the language that Ken Thompson and Dennis Ritchie were using to write the Unix operating system for the PDP-11 minicomputer (of which byte addressability was a feature)
* in 1972, large part of unix was rewritten in C. By 1973, with addition of struct types, most of Unix kernel was rewritten in C.
* Was standardized by ANSI (American National Standards Institute) in 1989 (ANSI C)
* Also standardized by ISO (International Organization for Standardization)

#### K&R C:
* 1978, Brian Kernighan and Dennis Ritchie publish The C Programming Language
* considered the lowest common denominator for C program instruction sets, for maximum portability with older compilers.

#### Standards:
* in 1983, ANSI formed a committee to establish standard spec of C.
* Produced C89, or ANSI C
* standard was adopted (with formatting changes) by ISO, leading to C90 (referring to same language specs)
* ISO is no the international C standard by which all others adopt
* most code written today conforms to c89
* c99 introduced in 1999, introduced inline functions, new data types (long long int, complex) variable length arrays , support for one line comments //

#### Characteristics:
* function parameters always passed by value
* pass-by-reference can be simulated by using pointers
* supports multiple assignments in one statement
* function return values can be ignored
* weakly enforced static typing - all data has type, can perform implicit conversions
* user defined typedef and compound types are possible
* structs allow related data to be packaged/assigned together
* union similar to struct, but total allocated memory is size of largest data member
* enumerated types available with enum
* strings are null terminated arrays of characters
* procedures are void returning functions
* preprocessor performs macro definition, source code file inclusion, and conditional compilation
* modularity - link files together using static and extern attributes
* i/o, string manipulation, math functions delegated to library routines
* order of operations can be a little screwy (x & 1 == 0 is not the same as (x & 1) == 0)

#### Operators:
* bitwise shifts: <<, >>
* bitwise logic: ~, &, |, ^
* boolean logic: !, &&, ||
* member selection: ., ->
* object size: sizeof
* reference and dereference: &, \*, [ ]
* sequencing: ,
* type conversion: (typename)

#### Pointers:
* dereference pointers to get value, or to call a function
* null pointers are 0, dereferencing them gets undefined behavior
* void pointers (void \*) are generic data type pointers, cant be dereferenced but easily converted to another type of pointer

#### C tools:
* gdb (debugger)
* valgrind (helps identify memory leaks)

#### Hello World:
```C
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}

// since c99+ specs, main is special and will implicitly return 0
```
